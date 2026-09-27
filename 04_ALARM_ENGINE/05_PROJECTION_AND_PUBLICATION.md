# Alarm Engine — Projection and Publication

Estado: **CURRENT / SOURCE V3 + LOCAL MATERIALIZATION READY / GLOBAL EFFECTIVE PLANNED**

Fuentes: `atlanticus@c8f23d91ae1cb817be55b4b812b22ffca518880e`; canonical previamente `58241ddb6db5adbd2e783c7ec9f456f1bda5a321`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.

## Capas separadas

```text
Alarm Source/Release (Blob: destino durable objetivo donde ya existe migración)
Alarm Configuration Projection (Cosmos puede servir a Materialization)
B.2 Runtime Configuration + B.2 Delivery Configuration (pareja READY)
Runtime Effective Configuration (futura adopción global durable)
Alarm Live Projection (estado actual y enriquecimiento exacto posterior)
Alarm Management Projection (historial, frontera separada)
```

La existencia de adaptadores no equivale a prueba de infraestructura. Source document type `ada_command_center_alarm_configuration_release`, schema v3: `AlarmConfigurationSnapshot(configuration, tool_dependencies: ToolDependencyManifest)` congela Cn en Rn. V2 queda SUPERSEDED sin decoder legacy contratado. `ProjectionRecord[AlarmConfigurationSnapshot]` conserva contenido y procedencia. Materialization utiliza la proyección activa adquirida mediante el adaptador Cosmos y revalida su identidad antes de publicar. **UNVERIFIED:** el flujo real Manager/Blob/Cosmos en infraestructura operativa.

El Confirmed Tool Catalog consolidado termina en Storage en su dominio migrado; no crear otra proyección de catálogo para reinterpretar Rn. B.2 usa el manifest Tool exacto de Rn y evidencia de qualification correspondiente.

## CURRENT: publicación local de Materialization

El proceso `scopes/ada-command-center/backend/processes/alarms-materialization` publica en:

```text
VOLUMEN_PATH/ada-command-center/alarms/materialization/
  ready.json
  versions/<result_id>/
    manifest.json
    runtime.json             # sólo READY
    delivery.json            # sólo READY
```

`result_id = "alarm-materialization-" + sha256(JSON canónico de source_key, projection_digest, qualification_digest)` según función compartida `materialization_result_id`. El manifest contiene: `document_type=ada_command_center_alarm_materialization_result`, `schema_version=1`, `source_key`, `result_id`, `status`, `resolution_key`, `provenance`, `findings`, `artifacts` y SHA256/tamaño por artefacto READY. La función compartida de lectura verifica identidad, esquema, procedencia, integridad de bytes y pareja Rn/Cn. `ready.json` incluye `source_key`, `result_id`, `resolution_key` y `manifest_sha256`; su formato es `ada_command_center_alarm_materialization_ready`, schema 1.

Las versiones publicadas son inmutables. El writer prepara una carpeta staging, valida el contenido y promueve la carpeta de versión mediante rename; después publica el puntero READY con `AtomicJsonStore`. El lector puede recuperar el READY publicado o leer un resultado exacto con `source_key`, `result_id` y hash. Un puntero corrupto o artefacto incoherente falla cerrado, **sin fallback silencioso** a otra versión. Un BLOCKED conserva manifest/findings sin Runtime ni Delivery ejecutables; no desplaza el READY anterior. Materialization **nunca** escribe EFFECTIVE.

El código de salida `CosmosAlarmMaterializationResultStore` está SUPERSEDED y fue eliminado del árbol actual; la **adquisición Cosmos de entrada se conserva**. El antiguo codec local duplicado del proceso fue sustituido por el codec compartido, sin adaptador legacy. El resolver B.2 sigue siendo una función pura, aunque su paquete también contiene el lector local compartido; no atribuir pureza I/O al paquete entero.

**Límite físico UNVERIFIED:** el código usa rename/FSync y fencing del job, pero no hay ensayo demostrado de la semántica del sistema de archivos/volumen real bajo varios hosts. No inferir atomicidad universal multi-FS o multiinstancia a partir de tests unitarios.

## CURRENT B1: identidad y planificación, no adopción

`AlarmConfigurationArtifactRef(source_key, result_id, manifest_sha256, resolution_key)` hace explícita la **materialización exacta**. Dos artefactos con la misma Rn/Cn pueden requerir planificación distinta si difiere la evidencia de qualification. `AlarmConfigurationRevision` la incorpora y se construye desde un resultado READY leído y un registro de evaluadores explícito. La construcción no sustituye la validación física de hashes del lector compartido.

## PLANNED: EFFECTIVE y publicación posterior

La adopción del Runtime deberá convertir una versión exacta READY en EFFECTIVE sólo tras reconciliación y autoridad durable. El formato, owner de publicación y recuperación del Effective Head **no están implementados**. No confundir el `ready.json` presente con un `effective.json` existente ni considerar que un plan B1 adelanta EFFECTIVE.

Engine y futuro Delivery deben usar exactamente la materialización adoptada; no elegir latest READY de forma independiente. Delivery/Live y Management son incrementos separados. No convertir esta regla de **lectura de configuración** en una afirmación general de que otras integraciones posteriores no pueden utilizar Cosmos.
