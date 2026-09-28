# Alarm Engine — Projection and Publication

Estado: **CURRENT / SOURCE V3, MATERIALIZATION LOCAL READY, WAL GLOBAL Y EFFECTIVE HEAD IMPLEMENTADOS / LIVE Y DELIVERY OPERACIONAL PLANNED**.

Fuente de implementación verificada: `atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`. Decisions: `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Reemplazo documental local, no publicación remota.

## Capas y owners separados

```text
Alarm Source/Release: Blob, destino durable objetivo en dominios migrados
Alarm Configuration Projection: adquisición Cosmos de entrada por Materialization
B.2 Materialization: pareja Runtime/Delivery READY, local e inmutable
Alarm Persistence: WAL global de adopción y proyección EFFECTIVE
Runtime: lectura exacta de artefacto EFFECTIVE y creación de sesión
Alarm Live Projection: estado actual, frontera posterior
Alarm Management Projection: historial de acciones, frontera posterior
History/Analytics: consumo posterior de hechos durables, no lógica del Engine
```

La presencia de adaptadores/protocolos no prueba un despliegue E2E real. La proyección de entrada `ProjectionRecord[AlarmConfigurationSnapshot]` conserva contenido y procedencia. Source `ada_command_center_alarm_configuration_release` schema 3 congela `ToolDependencyManifest(Cn)` junto con Rn; Source v2 está **SUPERSEDED**, sin decoder legacy aprobado. El Confirmed Tool Catalog consolidado termina en Storage en su dominio migrado; B.2 usa el manifest exacto de Rn y qualification pertinente, no consulta latest para reinterpretar releases.

## Materialization local READY — CURRENT

Proceso: `scopes/ada-command-center/backend/processes/alarms-materialization`. Publica:

```text
VOLUMEN_PATH/ada-command-center/alarms/materialization/
  ready.json
  versions/<result_id>/
    manifest.json
    runtime.json       (sólo READY)
    delivery.json      (sólo READY)
```

`result_id = "alarm-materialization-" + sha256(JSON canónico(source_key,projection_digest,qualification_digest))`. Manifest `ada_command_center_alarm_materialization_result`, schema 1: `source_key`, `result_id`, `status`, `resolution_key`, `provenance`, `findings`, `artifacts` y SHA256/tamaño por artefacto READY. `ready.json` tiene `ada_command_center_alarm_materialization_ready`, schema 1, source/result/key y SHA256 de manifest. La pareja READY Runtime/Delivery comparte Rn/Cn exactos.

El writer prepara carpeta staging, valida y promueve versión inmutable antes de reemplazar READY. BLOCKED mantiene manifest/findings, carece de Runtime/Delivery ejecutables y no desplaza READY previo. El lector compartido valida identidad, provenance, hashes/tamaño y pareja Rn/Cn; falla cerrado sin fallback a otras versiones. `LocalAlarmMaterializationReader` y codec compartido viven en `backend/alarms/materialization`. Sólo el **resolver** B.2 es puro; el paquete también contiene I/O.

La antigua salida de Materialization a Cosmos y su codec privado duplicado están **SUPERSEDED/retirados**; Cosmos continúa como entrada de proyección. **UNVERIFIED:** operación de infraestructura Manager/Blob/Cosmos real y semántica de rename/fencing en el volumen definitivo multi-host.

## Identidad y planificación B1 — CURRENT

`AlarmConfigurationArtifactRef(source_key,result_id,manifest_sha256,resolution_key)` identifica el resultado exacto aun con Rn/Cn iguales. `RuntimeLocalConfigurationReader.load_ready_candidate()` lee READY para planificar nuevas adopciones; `.load_exact_candidate()` lee una versión exacta. `build_alarm_configuration_revision()` construye `AlarmConfigurationRevision` y la sesión con `AlarmEvaluatorRegistry` explícito. La lectura no adopta por sí misma.

El planificador B1 cubre la unión de identidades definidas de origen y destino. Sus disposiciones `ADDED`, `ENABLED` y `REMOVED` de origen deshabilitado **no** prueban que el ejecutor anterior las soporte; ver `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md`.

## Adopción y EFFECTIVE — CURRENT B2a/B2b

La adopción es global, no por Rule ni priority group. Persistence contiene V1 para cero grupos y V2 para 1..N grupos relacionados por hash dentro del WAL existente, incluyendo cambios de identidad/materialización que no alteran hot state. `effective-head.json` está bajo `alarms/runtime/state/`; registra adopción, hash/posición del WAL, artefacto exacto y `effective_at`. Se publica después de materialización y se reconstruye únicamente desde el WAL validado.

`AlarmPersistence.read_effective_head()` verifica consistencia durable y snapshots; `RuntimeLocalConfigurationReader.load_effective_revision(persistence,evaluator_registry)` carga el artefacto exacto mediante el lector local y comprueba `source_key`, `result_id`, manifest SHA256 y Rn/Cn. `assert_current_effective()` permite revalidar la selección frente a una adopción posterior; `RuntimeEffectiveConfiguration` preserva cabeza y revisión seleccionada.

No existe `materialization/effective.json`: el nombre físico contratado es `runtime/state/effective-head.json`. La publicación READY nunca confiere EFFECTIVE. Un consumidor no puede elegir autónomamente latest READY. **La integración automática del job/executor con esta lectura continúa PLANNED B2c.**

## Consumidores posteriores, separados

Delivery/Live deben usar el mismo artefacto exacto que Runtime adoptó; no adelantar Rn/Cn ni reinterpretar Tool Catalog. Management Projection e History/Analytics consumirán hechos durables en sus propias fronteras. El Engine no debe importar lógica de dashboard y Web no debe leer WAL directamente.
