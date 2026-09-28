# Alarm Engine — Runtime Adoption and Effective Configuration

Estado: **CURRENT / B1 PLAN + B2a WAL GLOBAL V1/V2 + B2b EFFECTIVE HEAD Y LECTOR EXACTO; B2c EJECUTOR INTEGRADO PLANNED**.

Corte auditado en Git READ ONLY: `atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`; canonical de partida `be2c424c44648e6488daae36d410cf425eed02b8`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Gates locales comunicados por usuario, no CI/infraestructura E2E. Este archivo propone sustituir la descripción anterior que terminaba en B1, sin introducir contratos nuevos.

## 1. Invariantes congelados

```text
READY != EFFECTIVE
AlarmResolutionKey = (alarm_configuration_revision, confirmed_tool_catalog_revision)
AlarmConfigurationArtifactRef = (source_key, result_id, manifest_sha256, resolution_key)
AlarmArtifactRefSnapshot = (source_key, result_id, manifest_sha256,
                            alarm_configuration_revision, confirmed_tool_catalog_revision)
```

`RuntimeAlarmConfiguration` y `DeliveryAlarmConfiguration` de un mismo READY llevan igual Rn/Cn. Dos READY con Rn/Cn iguales pueden ser artefactos distintos si difiere qualification; pin exacto requiere `source_key`, `result_id` y hash de manifest además de Rn/Cn. Materialization sólo publica READY, no adopta. La autoridad de adopción es el **WAL del Engine**, no el puntero READY ni un artefacto `effective.json` junto a Materialization.

## 2. CURRENT: Materialization, lector y B1

`backend/alarms/materialization/local_reader.py` verifica hash/tamaño, schema, status READY, procedencia y pareja Runtime/Delivery; tiene lectura del READY publicado y lectura exacta. `RuntimeLocalConfigurationReader.load_ready_candidate()` puede adquirir candidatos para **planificación**; `.load_exact_candidate(result_id,manifest_sha256)` lee versión exacta. `build_alarm_configuration_revision(candidate,evaluator_registry)` crea `AlarmConfigurationRevision` con artifact ref exacto, identidades definidas y sesión ejecutable; el registry se inyecta y el builder no sustituye validación física del lector.

B1 `plan_configuration_adoption(source,target)` requiere mismo `source_key`, artefactos diferentes y cubre:

```text
source.defined_alarm_identities UNION target.defined_alarm_identities
```

Disposiciones actuales: `UNCHANGED`, `COMPATIBLE`, `ADDED`, `ENABLED`, `DISABLED`, `REMOVED`, `STRUCTURAL_RESET`, `REJECTED`. ADDED puede ser Rule target definida activa o deshabilitada; ENABLED es definida source deshabilitada a ejecutable target; DISABLED conserva Rule definida en target pero no ejecutable; REMOVED elimina identidad definida, incluso si source no era ejecutable; UNCHANGED permite ambas definidas deshabilitadas. `STRUCTURAL_RESET` cubre cambio de criticality; planner rechaza `priority_group`, `kind`, `evaluator_key` y ciertas mutaciones C1/C3. Cambios sólo de Delivery o de qualification pueden requerir adopción sin mutación de grupo.

`is_adoptable` sólo significa ausencia de `REJECTED`. `requires_execution_upgrade` detecta `ADDED`, `ENABLED` y `REMOVED` con source no ejecutable. No utilizar `is_adoptable` como gate suficiente para ejecutar el plan vigente.

## 3. CURRENT B2a: adopción global durable V1/V2

`backend/alarms/persistence/configuration_adoption.py`:

- V1 `ConfigurationAdoptionRecord`, schema `configuration-adoption-record.v1`: singleton global sin group commits; `adoption_id`, `previous_artifact_ref` opcional, `target_artifact_ref`, UTC `effective_at`/`committed_at`, hash canónico. No crear snapshot o prioridad sintética.
- V2 `ConfigurationAdoptionRecordV2`, schema `configuration-adoption-record.v2`: hereda identidad de adopción y añade `group_commits` 1..N, referencias exactas ordenadas `GroupCommitReference(priority_group,commit_id,record_hash)`. Los grupos se registran antes del adoption record final dentro del mismo batch de WAL; referencias, hash, revisiones objetivo y mismo segmento horario UTC se validan antes de confirmación.
- V1 sigue siendo contrato CURRENT para cero grupos, no decoder legacy transitorio. V2 no redefine el significado de V1 ni de commits normales.

`AlarmPersistence.commit_adoption(record, group_records=(), assert_authority, fenced_mutation)` valida cadena global y estado previo; soporta V1 sin grupos y V2 con grupos. `EngineJournal` valida ambas versiones, cadenas de grupos, continuidad y referencias exactas. `read_durable_adoptions()` permanece separado de `read_durable_records()` de grupos. Primer adoption sobre histórico durable group-only anterior falla cerrado: **migración explícita no implementada**, sin autocorrección oportunista.

Recovery: si falla antes de Durable Head, descarta cola WAL no confirmada; después de Durable Head, recupera registros sin reevaluación; V2 no avanza Materialized Head a la mitad de un grupo transaccional y reintenta materializaciones idempotentemente. Hay garantía de confirmación durable agrupada, **no** snapshot raw externo atómico entre múltiples archivos.

## 4. CURRENT B2b.1: Effective Head recuperable

`backend/alarms/persistence/effective_head.py` implementa `AlarmEffectiveConfigurationHead`, schema `alarm-effective-head.v1`:

```text
adoption_id
adoption_record_hash
adoption_position: JournalPosition
target_artifact_ref: AlarmArtifactRefSnapshot
effective_at
```

Layout contratado:

```text
VOLUMEN_PATH/ada-command-center/alarms/runtime/state/
  journal-head.json
  effective-head.json
  groups/<priority_group>.json
```

El Effective Head es proyección del **último adoption record durable**; se publica después de Materialized Head con fencing. `AlarmPersistence.read_effective_head()` exige journal alineado, reconstruye expectativa a partir de la región durable, exige igualdad con proyección física y verifica snapshots de grupos durables. Falta de adopción puede producir `None`; ausencia, corrupción u obsolescencia de proyección con adopción durable requieren recovery antes de lectura efectiva. Recovery reconstruye tras caída incluso con `durable == materialized`; corrupción del WAL y discrepancias de snapshots fallan cerradas. Sin adopción durable, una proyección EFFECTIVE física huérfana es corrupción, no un fallback.

Esta proyección no es segundo journal ni owner de publicación de Materialization; no se lee `ready.json` para decidir qué versión restaurar.

## 5. CURRENT B2b.2: carga exacta desde Runtime

`processes/alarms-runtime/local_configuration.py` incorpora:

```text
RuntimeEffectiveConfiguration(effective_head, revision)
RuntimeLocalConfigurationReader.load_effective_revision(persistence, evaluator_registry)
RuntimeLocalConfigurationReader.assert_current_effective(persistence, selected)
RuntimeEffectiveConfigurationError
```

La lectura exige instancia real de `AlarmPersistence` sobre el **mismo VOLUMEN_PATH** y registry de evaluadores explícito. Obtiene cabeza efectiva validada, compara `source_key`, lee `result_id` y manifest SHA256 exactos, construye revisión y compara todos los campos del pin, incluida Rn/Cn. Una segunda lectura de EFFECTIVE descarta selección obsoleta; `assert_current_effective` permite otro chequeo posterior.

Si no existe ninguna adopción efectiva devuelve `None`; no interpreta READY como bootstrap automático. Ante inconsistencia, versión inexistente, corrupción, wrong source o adopción posterior, falla cerrado. **No** altera por sí mismo `job_composition.py` ni `adoption_execution.py`; la existencia del lector no demuestra que el job actual lo utilice ni que Runtime ya efectúe un flujo completo de adopción.

## 6. CURRENT pero PARCIAL: ejecutor anterior

`AlarmConfigurationAdoptionExecutor.execute(...)` en `adoption_execution.py` valida tipo/UTC, `plan.is_adoptable` y alineación del journal; agrupa cada cambio distinto de `UNCHANGED` mediante `plan.source.plan_for(identity)`. Esa búsqueda falla si la Rule no tiene source plan ejecutable, por ejemplo ADDED, ENABLED y REMOVED desde source definida pero deshabilitada. `is_adoptable=True` no impide ese fallo. Para grupos que sabe preparar llama a `reconcile_group_configuration` y `materialize_group_commit`, y finalmente `composition.commit_batch` de grupos; cuando no hay grupos devuelve sin `commit_result`. **No llama a `AlarmPersistence.commit_adoption` V1/V2 ni publica autoridad global como parte del flujo ejecutor.**

`AlarmRuntimeJobComposition.recover()` usa el recovery de composición y obliga a completarlo antes de `iteration()`. Comprueba `head.aligned`, pero el ejecutor inyectado y su selección EFFECTIVE operacional completa no se han conectado en este hito. No confundir gates de recovery con orquestación B2c terminada.

## 7. PLANNED B2c: siguiente frontera, sin decisión nueva aquí

Primero debatir **B2c.1, semántica y ejecución segura de las disposiciones ya contratadas por B1**. Revisar grupos origen/destino, source/target definido/ejecutable, casos sin grupos y ausencia de duplicados; contrastar `reconcile_group_configuration` y los tests existentes antes de escribir código. Acordar comportamiento concreto, fallos y casos de regresión. No inventar un plan adicional o un registro nuevo sólo por comodidad.

Sólo después abordar **B2c.2, integración del ejecutor con los contratos físicos ya disponibles**: V1 para 0 grupos, V2 para 1..N, pin exacto de target, checks de head/fencing/recovery y lectura EFFECTIVE. No duplicar journal, puentes legacy ni autorizar latest READY como fuente de ejecución. La política de `evaluator_key`/`kind` y migración de `priority_group` según decisions permanece **OPEN / CONFLICT** y requiere acuerdo explícito fuera de un parche incidental.

## 8. Otros OPEN, fuera del presente incremento

- Reconcile sobre `ManagementEffect` vivo para cambios de timer y Special Conditions.
- Provenance exacta `resolution_key_at_start` en occurrence, sólo si se confirma contrato/alcance.
- First bootstrap operacional completo y migración condicional desde histórico group-only, no asumir datos legados concretos.
- Qualification de evaluadores y Tools GREEN reales; Cosmos/Blob y volumen multi-host físico UNVERIFIED.
- Fuentes/evaluadores reales, Delivery/Live, Management Capture, History/Analytics y contratos de evidencia/retención: frentes separados.

## 9. Evidencia y frontera de cierre

B2a.1/B2a.2/B2b.1/B2b.2 figuran en el HEAD verificado con PASS local de pruebas, Ruff y builds delimitados en `08_QUALIFICATION_BASELINE.md`. Ninguno demuestra funcionamiento operacional E2E del executor B2c ni CI sobre un checkout limpio. El presente cambio es **sólo documental**, destinado a sustituir el relato B1 anterior sin retroceder a diseños ya SUPERSEDED.
