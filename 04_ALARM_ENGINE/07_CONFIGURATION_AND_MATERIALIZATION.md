# Alarm Engine — Configuration and Materialization

Estado: **CURRENT / SOURCE V3 + B.2 RESOLVER + LOCAL PUBLICATION + RUNTIME EXACT READER + B1**

Corte: `atlanticus@c8f23d91ae1cb817be55b4b812b22ffca518880e`; canonical previo `58241ddb6db5adbd2e783c7ec9f456f1bda5a321`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Los tests citados proceden de logs locales del usuario; el estado de código se contrastó en Git modo lectura.

## Invariantes previos congelados

```text
LATEST SAVED = LATEST VALID_AT_SAVE
VALID_AT_SAVE != READY != EFFECTIVE
INVALID != REMOVED
DISABLED != INVALID
DISABLED != REMOVED
TRACE_ONLY != REMOVED
READY != EFFECTIVE
```

B.1 valida guardado/publicación; B.2 realiza la qualification/resolución externa. Resolver, adquisición/publicación del proceso, lectura compartida y Runtime Adoption tienen responsabilidades diferentes.

## Source + manifest Tool exacto — CURRENT

```text
AlarmConfiguration(rules, messages)
AlarmConfigurationSnapshot(configuration, tool_dependencies: ToolDependencyManifest)
source document_type: ada_command_center_alarm_configuration_release
source schema_version: 3
AlarmResolutionKey(Rn, Cn): alarm_configuration_revision + confirmed_tool_catalog_revision
```

Source v2 está SUPERSEDED y no se conserva decoder legacy. El manifest de Tool del release Rn cubre referencias definidas de Rules activas/inactivas, escalones habilitados/deshabilitados y visual targets; incluye procedencia y estructura exacta Cn. Manager mantiene la revisión como sidecar del draft y comprueba drift antes de Validate/Publish. No consultar latest Tool Catalog para reinterpretar la versión ya publicada.

Existen codec/builder y adaptadores Local/Cosmos de `ProjectionRecord[AlarmConfigurationSnapshot]`; Materialization obtiene la proyección activa desde Cosmos, fija identidad, carga evidencia JSON controlada y revalida digest/proyección antes de persistir. La existencia de adaptadores no valida la operación E2E de infraestructura real.

## Resolver B.2 — CURRENT

```text
Alarm Configuration Rn + ToolDependencyManifest(Cn)
+ ToolReconciliationQualification + EvaluatorQualificationCatalog
-> resolve_alarm_configuration(...)
-> AlarmConfigurationResolution(READY | BLOCKED)
```

Toda Rule definida necesita evaluator cualificado por `(family_key, evaluator_key)` y todas las referencias Tool deben existir y estar GREEN, incluso cuando la Rule/step esté inactiva/deshabilitada. B.2 valida routing, Messages y visual targets. `READY` genera `RuntimeAlarmConfiguration` y `DeliveryAlarmConfiguration` de la **misma** `AlarmResolutionKey`; `BLOCKED` emite hallazgos bloqueantes sin artefactos ejecutables. El resolver sigue siendo puro; el paquete `alarms/materialization` integra adicionalmente codec y lector local, que sí realizan lectura I/O.

Strict routing congelado: `PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END`, exclusivamente nivel siguiente; sin saltos, retrocesos ni same-tier. Strategic terminal; C3 sólo origen; C1 inmediato; C2 delays positivos y offsets acumulados desde `occurrence.started_at`; los steps disabled no ejecutan pero sus referencias se cualifican. Strategic sólo routing, no visual target contratado.

## Proceso ejecutable y publicación — CURRENT

`scopes/ada-command-center/backend/processes/alarms-materialization` permanece en versión **1.0.0** predespacho y usa `atlanticus.runtime.execute_job`. `composition.py` construye el acquirer sobre `CosmosAlarmConfigurationProjectionStore`; `qualification.py` conserva el proveedor controlado `JsonFileAlarmQualificationProvider`; `publication.py` usa `LocalAlarmMaterializationResultStore`. La salida Cosmos monolítica fue sustituida limpiamente; el codec se compartió en `backend/alarms/materialization/codec.py`, sin duplicación local anterior.

Layout físico vigente:

```text
VOLUMEN_PATH/ada-command-center/alarms/materialization/
  ready.json
  versions/<result_id>/manifest.json
  versions/<result_id>/runtime.json          (sólo READY)
  versions/<result_id>/delivery.json         (sólo READY)
```

`result_id` incorpora `source_key`, digest de proyección y digest de qualification. Manifest schema 1 contiene procedencia exacta, `resolution_key`, findings e inventario de hashes/tamaños. El writer publica la carpeta completa de versión antes de promover READY; detecta divergencias de identidad/contenido, hace retry/idempotencia cuando corresponde y no promueve BLOCKED. Un resultado BLOCKED conserva diagnóstico sin archivos ejecutables y sin reemplazar READY previo. Las versiones ya publicadas son inmutables y el lector falla cerrado ante corrupción. Atomicidad física en FS/host concreto y multiinstancia: **UNVERIFIED**.

## Lector compartido y consumo Runtime — CURRENT

`backend/alarms/materialization/local_reader.py` expone `LocalAlarmMaterializationReader`, `ReadyAlarmMaterialization` y `materialization_root`. `backend/processes/alarms-runtime/local_configuration.py` expone `RuntimeLocalConfigurationReader.load_ready_candidate()` y `.load_exact_candidate(result_id, manifest_sha256)`; ninguna llamada convierte READY en EFFECTIVE. Carga exacta valida source, hash del manifest, integridad de Runtime/Delivery, `resolution_key` compartida y semántica READY.

## Incremento B1 — CURRENT en main, CLOSED para validación local

`AlarmConfigurationArtifactRef` incorpora `source_key`, `result_id`, `manifest_sha256` y `resolution_key`. En Runtime, `build_alarm_configuration_revision(candidate, evaluator_registry)` vincula una candidata READY previamente obtenida al `AlarmEvaluatorRegistry` explícito y produce `AlarmConfigurationRevision(artifact_ref, defined_alarm_identities, session)`. `plan_configuration_adoption` cubre `source.defined_alarm_identities UNION target.defined_alarm_identities`; acepta diferente `result_id` aun con igual Rn/Cn y rechaza misma identidad de artefacto, conflicto de digest para un mismo result_id y diferentes source_key. Las disposiciones nuevas `ADDED` y `ENABLED` existen **en planificación**.

**Frontera de seguridad:** `plan.is_adoptable` puede ser verdadero y `plan.requires_execution_upgrade` también. El ejecutor anterior (`adoption_execution.py`) no fue modificado en B1; no afirmar que ejecuta ADDED/ENABLED ni eliminación de Rules deshabilitadas. No hay aún adopción global durable ni Effective Head, incluso para cambios que no alteran hot state. Política de cambios de evaluator/kind/priority_group mantiene rechazos actuales, aunque decisiones históricas registran semánticas objetivo diferentes: **conflicto visible, no resuelto**.

## Evidencia acotada y límites

Logs de pruebas locales del usuario, posteriores a la aplicación de A y B1:

- A: lectura, publicación y tests completos de los tres componentes, lint/format/wheels PASS; `atlanticus@9693e2b...` integró el incremento.
- B1: Materialization compartido `62 PASS`, Runtime `40 PASS` tras corrección Ruff, Materialization Process `43 PASS`; checks de formato/lint PASS y wheels relevantes construidos. Main `c8f23d9...` contiene el código B1; **no** se repitió CI/pytest en un checkout limpio de ese SHA en este cierre.
- Infraestructura Cosmos/Blob, qualification automática real, volumen físico multi-host, Runtime Adoption EFFECTIVE y Delivery/Live: **UNVERIFIED/PLANNED**.

## Siguiente frontera, sin abrirla en este cierre

Diseñar y acordar el contrato de adopción **global durable** y la evolución explícita del ejecutor para las disposiciones nuevas sobre el WAL existente. Definir primero seguridad/recovery y `requires_execution_upgrade`; no agregar journal paralelo, grupo artificial, fallback a latest READY, persistencia EFFECTIVE prematura ni compatibilidad legacy. Implementar únicamente tras consenso en otro chat; Delivery queda separado.
