# Alarm Engine — Runtime Adoption and Effective Configuration

Estado: **CURRENT — B1/B2a/B2b y ejecución B2c implementados en alcance verificado; fuentes B2c.5c y ejemplo B2c.5d integrados; wiring productivo B2c.6 PLANNED**. Corte 2026-09-28: `atlanticus@a799dc15105d3e037f36ab77129ef0cfa8999013`, `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`, canonical anterior `46877f174513b2475f17b7dc739cd43951fa4ed0`.

Esta actualización sustituye la sección histórica que trataba todo B2c como futuro. Sólo afirma lo contrastado en código/logs: no declara E2E físico ni composition operacional ya montada.

## 1. Invariantes frozen

```text
READY != EFFECTIVE
AlarmResolutionKey = (alarm_configuration_revision, confirmed_tool_catalog_revision)
AlarmConfigurationArtifactRef = (source_key, result_id, manifest_sha256, resolution_key)
AlarmArtifactRefSnapshot = (source_key, result_id, manifest_sha256,
                            alarm_configuration_revision, confirmed_tool_catalog_revision)
WAL -> DURABLE HEAD -> SNAPSHOTS -> MATERIALIZED HEAD
```

Materialization publica una pareja Runtime/Delivery exacta READY o BLOCKED, no adopta. Dos resultados con Rn/Cn iguales pueden diferir por qualification: el pin incluye source/result/hash. El **WAL del Engine** es autoridad de EFFECTIVE, `effective-head.json` es proyección reparable derivada del último adoption durable. No usar latest READY como sustituto de EFFECTIVE en recovery; no introducir journal adicional.

## 2. B1 — candidato exacto y plan CURRENT

El lector compartido `backend/alarms/materialization/local_reader.py` valida manifest, hashes, status, procedencia y ambos artefactos. `RuntimeLocalConfigurationReader` puede leer READY para planificar o una versión exacta por result/hash. `build_alarm_configuration_revision(candidate,evaluator_registry)` construye revision/session con registry **explícito**; no fabrica evaluadores faltantes.

`plan_configuration_adoption` considera identidades definidas source UNION target y produce `UNCHANGED`, `COMPATIBLE`, `ADDED`, `ENABLED`, `DISABLED`, `REMOVED`, `STRUCTURAL_RESET` o `REJECTED`. El planner actual rechaza cambios de `priority_group`, `kind`, `evaluator_key` y ciertas mutaciones C1/C3. `is_adoptable` describe ausencia de REJECTED; no sustituye comprobación de capacidad/estado de ejecución. Cambios sólo Delivery/qualification siguen requiriendo adopción del artefacto cuando corresponda.

## 3. B2a — WAL V1 y V2 CURRENT, no legacy

- V1 `ConfigurationAdoptionRecord`, `configuration-adoption-record.v1`: adopción global sin grupos, sin snapshot ficticio; identidad exacta, revision target y tiempos/hashes validados.
- V2 `ConfigurationAdoptionRecordV2`, `configuration-adoption-record.v2`: 1..N grupos y `GroupCommitReference` ordenadas, hash/identidad exactos; registros de grupo antes del adoption final en el mismo lote de WAL.
- `AlarmPersistence.commit_adoption` valida estado previo y autoridad, confirma cadena durable y permite recovery/replay sin reevaluación posterior al Durable Head. `read_durable_adoptions` permanece separado de registros de grupo; versiones V1/V2 son ambas **CURRENT**.
- No hay garantía MVCC para lectores externos arbitrarios de snapshots multiarquivo; la barrera agrupada sigue siendo Durable/Materialized. Primer adoption sobre histórico durable group-only anterior sin migración explícita falla cerrado. No añadir migración condicional sin inventario real.

## 4. B2b — Effective Head CURRENT

`AlarmEffectiveConfigurationHead` (`alarm-effective-head.v1`) contiene adoption ID, hash, posición WAL, target ref y tiempo efectivo. Layout lógico ya existente:

```text
VOLUMEN_PATH/ada-command-center/alarms/runtime/state/
  journal-head.json
  effective-head.json
  groups/<priority_group>.json
```

`AlarmPersistence.read_effective_head()` requiere journal alineado; compara región durable con la proyección física y los snapshots. Recovery recupera proyección tras caída; corrupción WAL, discrepancias de estado u orphan EFFECTIVE fallan cerradas. El lector Runtime exige **mismo VOLUMEN_PATH y `AlarmPersistence` real**, obtiene ref exacta y reconstruye revisión mediante registry explícito; `assert_current_effective` detecta que una selección se volvió obsoleta. No confundir lector con la publicación READY ni con un consumidor Web.

## 5. B2c — ejecutor y selección operativa CURRENT en main

El diagnóstico anterior del canonical (ejecutor sólo hace `composition.commit_batch` de grupos y omite V1/V2) pertenecía a un checkpoint previo. **SUPERSEDED como descripción del código actual**: `adoption_execution.py` contiene preparación, bootstrap y ejecución con `composition.durability.commit_adoption`, más comprobación de confirmación; el job `configured_iteration.py` selecciona EFFECTIVE exacto o candidato READY, espera 30 segundos si no existe configuración inicial ejecutable, confirma bootstrap/adopción, pinnea la revisión efectiva y ejecuta el ciclo. Los casos no ejecutables de READY inicial no deben activar una configuración ficticia; un job fijado no adopta cada nueva revisión durante la misma sesión.

`process.py` ya expone `build_alarm_runtime_process(runtime_configuration,definition,source_key,evaluator_registry,source_loader,technical_evidence_contract,occurrence_id_factory,episode_id_factory,commit_time_provider,runtime_artifact_version,clock,... )`. `AlarmOperationalCycleRunner` usa la sesión fijada, carga una iteración a tiempo UTC de segundo y ejecuta `AlarmOperationalCycle` sobre Core/Persistence. Mantener autoridad de lease/fencing y prohibición de segundo commit del mismo grupo en un mismo segundo. **No inferir** de esta API que exista en producción un `__main__`/bootstrap que cablee dependencias operacionales ya calificadas: es el tema de B2c.6.

## 6. B2c.5c — Data Requirements y lector de fuentes CURRENT

En `session.py`, `AlarmEvaluatorContract` acepta `requirements: tuple[DataRequirement,...]` o `requirements_resolver` (mutuamente excluyentes). La sesión resuelve entradas, conserva requisitos exactos por alarma y arma `DataLoadPlan` consolidado. El planner consolida vistas fuente/partición y ventanas; el adapter/contexto entrega exclusivamente lo requerido por cada alarma. Los fallos de fuente o schema se atribuyen a alarmas afectadas y no deben transformarse en INACTIVE.

`source_reader.py` ofrece `AlarmRoutedDatasetReader` y `build_alarm_source_adapter`; utiliza registro y aplicaciones de fuentes operacionales existentes, lector Parquet y filtros temporales. En el corte B2c.5c se verificaron **14 fuentes** y **18 combinaciones fuente/partición**: PI_INTERPOLATED LATEST/DAILY/MONTHLY, PI_RECORDED DAILY/MONTHLY, seis Dispatch SHIFT, Dispatch STD_TRUCK LATEST, Blockgrade SHIFT, tres Remanentes LATEST y FABRICA_PLANES DAILY/WEEKLY. FABRICA_KPIS y Meteodata no se incorporaron a ese registro.

El puerto genérico `requirements_resolver` es una **capacidad implementada**; no borrarlo ni reinterpretarlo como obligación de derivar fuentes desde parámetros Web.

## 7. B2c.5d — catálogo de evaluadores, ejemplo y parámetros CURRENT

La regla acordada para desarrollar nuevas lógicas del catálogo es: el desarrollador **es dueño de DataRequirement manual** y el evaluador consume parámetros de negocio **sólo si el autor los utiliza**. No implementar esquema universal de obligatoriedad, catálogos de parámetros ni vínculo automático columnas/ventanas <- parámetros Web. El `EvaluationContext.parameters` puede ser `{}` y sus tipos permitidos siguen siendo `str|float|bool`; un autor puede validar campos dentro de su propia lógica, asumiendo las reglas existentes de captura de errores del Engine.

Cada lógica entrega un `AlarmEvaluatorContract` vinculando código, `(family_key,evaluator_key)` y requisitos. `AlarmEvaluation` requiere ACTIVE/INACTIVE + `EvidenceSnapshot` JSON-compatible o ERROR + `EvaluationError`. `execute_evaluator` comprueba identidad y timestamp y aísla excepciones. No modificar occurrence, episode, priority, management ni WAL desde `evaluator.py`; Core y el ciclo son sus propietarios.

**Distribución efectiva en `a799dc1`:**

```text
processes/alarms-runtime/src/ada_command_center/processes/alarms_runtime/
  catalog/__init__.py
  catalog/registry.py                       # CURRENT: AlarmEvaluatorRegistry(contracts=())
  catalog/examples/__init__.py
  catalog/examples/threshold/__init__.py    # contrato importado explícitamente en tests
  catalog/examples/threshold/requirements.py
  catalog/examples/threshold/evaluator.py
```

Espejo pedagógico equivalente bajo `commented/.../catalog/...`. El ejemplo declara PI_INTERPOLATED/DAILY, `temperature` FLOAT y `TimeWindow(4,HOURS)`; el parámetro `limit` es opcional (default demostrativo 80.0). `EvidenceSnapshot('mina.threshold','v1',payload)` guarda fuente/partición/columna/ventana/valor/límite/comparación. Si no hay muestra finita, retorna ERROR de QUALITY. La suite también verifica que valores de parámetros incompatibles pueden terminar en ERROR de EVALUATOR **sin** establecer validador genérico.

El ejemplo en `catalog/mina/threshold` y su inclusión automática en `registry.py` son **SUPERSEDED**. El registro productivo queda explícitamente **vacío** hasta registrar lógicas reales aprobadas. Las 7 pruebas específicas, 443 de regresión, Ruff PASS y 40 archivos formateados fueron comunicados por el usuario antes del SHA final y éste se comprobó remotamente. CI y wheel tras traslado: UNVERIFIED.

## 8. OPEN explícitos, fuera de la composición B2c.6

- Decisions B.1 desea `evaluator_key` y `kind` compatibles y `priority_group` migrable origen/destino; `adoption.py` actual rechaza. **OPEN / CONFLICT**, no arreglar incidentalmente.
- Real qualification Tool/evaluator, despliegue con fuentes físicas, recuperación multi-host y CI en checkout limpio: **UNVERIFIED**.
- Web `max_duration_hours` sólo numérico y Domain int 1..12; sin política `hasta fin del turno`: **OPEN** separado de composición. El tiempo efectivo UTC del Core no define por sí mismo nueva opción de autoría.
- Live Delivery, Management Capture, History/Analytics, provenance `resolution_key_at_start`, reappearance de management vivo y migración histórica condicional: frentes separados.
- Metadatos Python `==3.14.2` en paquetes Command Center vs target Project 3.14.7: pendiente transversal.

## 9. Siguiente frontera B2c.6 — sólo composición

Auditar primero el bootstrap, entrypoints/scripts y puertos ya existentes. Decidir dónde y cómo se construye el registro de **lógicas reales** sin ejemplos, se crea `build_alarm_source_adapter` con registry/rutas/PI provider actuales y se inyectan factories/clock/persistencia existentes a `build_alarm_runtime_process`. Probar recorrido controlado sin ambiente físico. No crear código, nuevos contratos, procesos o servicios remotos hasta consenso y autorización de implementación incremental.
