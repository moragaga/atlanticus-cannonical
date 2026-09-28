# Alarm Engine — Configuration and Materialization

Estado: **CURRENT / SOURCE V3 + PURE B.2 + PUBLICACIÓN LOCAL + LECTOR EXACTO + B1 + LECTURA EFFECTIVE B2b**.

Corte: `atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`, `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Tests citados provienen de logs locales del usuario; la existencia de código se contrastó en Git READ ONLY.

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

B.1 valida persistencia del release; B.2 realiza qualification/resolución externa; el proceso Materialization adquiere/publica; el lector compartido verifica versiones; B1 planifica; B2a/B2b persisten/leen adopción. Son responsabilidades distintas.

## Source + manifest Tool exacto — CURRENT

```text
AlarmConfiguration(rules,messages)
AlarmConfigurationSnapshot(configuration, tool_dependencies: ToolDependencyManifest)
source document_type: ada_command_center_alarm_configuration_release
source schema_version: 3
AlarmResolutionKey(Rn,Cn): alarm_configuration_revision + confirmed_tool_catalog_revision
```

Source v2 está **SUPERSEDED** sin decoder legacy. Tool manifest de Rn cubre referencias definidas de Rules activas/inactivas, escalones enabled/disabled y visual targets con procedencia Cn congelada. Manager conserva revisión en sidecar del draft y verifica drift en Validate/Publish. Materialization obtiene proyección activa de Cosmos, fija la identidad, carga qualification JSON controlada, revalida su digest/procedencia y no usa latest Tool Catalog para reescribir el release.

Los adaptadores Local/Cosmos y sus contratos existen; su operación de infraestructura real no fue objeto de este gate.

## Resolver B.2 — CURRENT

```text
Alarm Configuration Rn + ToolDependencyManifest(Cn)
+ ToolReconciliationQualification + EvaluatorQualificationCatalog
-> resolve_alarm_configuration(...)
-> AlarmConfigurationResolution(READY | BLOCKED)
```

Toda Rule definida exige evaluator cualificado `(family_key,evaluator_key)` y referencias Tool válidas GREEN aun en Rules/steps deshabilitados. El resolver valida routing, Messages y visual targets. READY genera `RuntimeAlarmConfiguration` y `DeliveryAlarmConfiguration` con **misma** Rn/Cn; BLOCKED emite findings sin artefactos ejecutables.

Strict routing CURRENT: `PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END`, sólo nivel inmediato siguiente, sin saltos/same-tier/regresión. Strategic es terminal; C3 sólo origen; C1 inmediato; C2 con delays positivos y offsets acumulados desde `occurrence.started_at`. Steps disabled no ejecutan pero sus referencias se cualifican. Strategic sólo routing, no visual target contratado.

El resolver es puro, no todo el paquete, que ahora contiene codec y lector con I/O.

## Proceso ejecutable y publicación local — CURRENT

`scopes/ada-command-center/backend/processes/alarms-materialization` mantiene versión predespacho `1.0.0`, usa `atlanticus.runtime.execute_job`, adquiere con `CosmosAlarmConfigurationProjectionStore`, usa `JsonFileAlarmQualificationProvider` controlado y publica con `LocalAlarmMaterializationResultStore`. La salida anterior Cosmos y codec privado quedaron **SUPERSEDED**, no dual-write.

```text
VOLUMEN_PATH/ada-command-center/alarms/materialization/
  ready.json
  versions/<result_id>/manifest.json
  versions/<result_id>/runtime.json     (READY)
  versions/<result_id>/delivery.json    (READY)
```

`result_id` utiliza `source_key`, digest de proyección y digest de qualification. Manifest schema 1 conserva identidad, provenance, `resolution_key`, findings e inventario con hashes/tamaños. Una versión se valida y publica antes de promover READY. BLOCKED mantiene diagnóstico pero no archivos ejecutables ni reemplaza READY anterior. Versiones publicadas inmutables; lector falla cerrado ante corrupción. Atomicidad física real multi-host: **UNVERIFIED**.

## Lector compartido, lector Runtime y B1 — CURRENT

`backend/alarms/materialization/local_reader.py` ofrece `LocalAlarmMaterializationReader`, `ReadyAlarmMaterialization` y `materialization_root`. `RuntimeLocalConfigurationReader` conserva `load_ready_candidate()` para planificación y `load_exact_candidate(result_id, manifest_sha256)`; ninguna convierte READY en EFFECTIVE.

`AlarmConfigurationArtifactRef` fija `source_key`, `result_id`, hash del manifest y Rn/Cn. `build_alarm_configuration_revision(candidate,evaluator_registry)` produce `AlarmConfigurationRevision` y sesión con registry explícito; presupone la validación física de la candidata por el lector. B1 permite distintas materializaciones con igual Rn/Cn y planifica sobre `source.defined_alarm_identities UNION target.defined_alarm_identities`, incluso Rules deshabilitadas. ADDED/ENABLED en el plan no implican ejecución garantizada.

## B2a/B2b — CURRENT, frontera posterior a Materialization

**B2a.1/B2a.2:** `backend/alarms/persistence` registra adopción global durable V1 sin grupos o V2 con 1..N grupos y referencias exactas, sobre el WAL ya existente. **B2b.1:** `runtime/state/effective-head.json` es proyección reconstruible desde la última adopción durable y snapshots verificados. **B2b.2:** `RuntimeLocalConfigurationReader.load_effective_revision(persistence,evaluator_registry)` carga la revisión exacta EFFECTIVE; `RuntimeEffectiveConfiguration` preserva su `effective_head` y `revision`; `assert_current_effective()` detecta cambios posteriores de selección.

No escribir EFFECTIVE desde Materialization. No leer `ready.json` como si fuera autoridad del Engine. No volver a Cosmos para reinterpretar Runtime/Delivery ya materializados. **B2c sigue pendiente:** el ejecutor y el job no están conectados todavía al commit global V1/V2; `is_adoptable` no garantiza ejecución de todas las transiciones.

## Evidencia delimitada y límites

Incrementos previos: A local reader y publicación; B1 artifact exacto/planificación, todos con gates locales indicados en `08_QUALIFICATION_BASELINE.md`. Incrementos del presente corte: B2a.1, B2a.2, B2b.1, B2b.2 con tests/Ruff y builds locales; ficheros integrados en `main` en los checkpoints verificados. No convertir estas pruebas en verificación CI limpia, volumen multi-host, Cosmos/Blob productivo ni ejecutores GREEN reales. Ver `11_SOURCE_LEDGER.md`.

## Siguiente frontera

**B2c exclusivamente:** revisar qué disposiciones B1 soporta realmente `adoption_execution.py`, luego conectar el ejecutor con V1/V2/EFFECTIVE preservando recovery y fencing. No ampliar Materialization, Delivery ni contratos Core fuera de un acuerdo explícito.
