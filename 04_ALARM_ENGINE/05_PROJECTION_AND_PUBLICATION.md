# Alarm Engine — Projection and Publication

Estado: **CURRENT / SOURCE V3 AND PROJECTION ADAPTERS / MATERIALIZATION LOCAL OUTPUT DECIDED, NOT IMPLEMENTED**

## Capas que no deben fusionarse

```text
Alarm Source/Release (Blob durable objetivo en dominios migrados)
Alarm Configuration base/operational Projection (Cosmos puede servirla)
B.2 Runtime Configuration (candidato READY)
B.2 Delivery Configuration (mismo candidato READY)
Runtime Effective Configuration (adopción posterior)
Alarm Live Projection (estado operacional actual)
Alarm Management Projection (historial de acciones)
```

## Source y proyección — CURRENT

`AlarmConfigurationSnapshot(configuration, tool_dependencies: ToolDependencyManifest)`; `source document_type=ada_command_center_alarm_configuration_release`, `schema_version=3`. V2 **SUPERSEDED**, sin decoder legacy.

`ProjectionRecord[AlarmConfigurationSnapshot]` conserva release, snapshot íntegro y evidencia Tool congelada. Existen codec, builder y stores Local/Cosmos de Alarm Configuration. La lectura operativa real contra Cosmos y la publicación del productor aún no se han probado E2E: **UNVERIFIED**. El hecho de existir un adapter no demuestra disponibilidad de infraestructura.

No reinterpretar `Rn/Cn` mediante latest Tool Catalog. El Confirmed Tool Catalog consolidado termina en Storage y no necesita proyección adicional a Command Center Cosmos.

## Materialization — decisión nueva

Materialization es la frontera de lectura de la proyección operativa en Cosmos. Valida el candidato y qualifications exactos e invoca el resolver puro B.2. **Su salida de configuración pasa al volumen compartido**, no a otro documento Cosmos.

```text
READY   -> Runtime + Delivery [misma AlarmResolutionKey] + evidencia local
BLOCKED -> findings/diagnóstico sin publicar Runtime ni Delivery ejecutables
```

La versión actual `backend/processes/alarms-materialization==0.2.1` implementa, en cambio, `CosmosAlarmMaterializationResultStore`, con resultado monolítico Cosmos. Es **CURRENT en código / SUPERSEDED como diseño**; deberá reemplazarse limpiamente. Los nombres `runtime.json`, `delivery.json`, `manifest.json`, `ready.json`, `effective.json` y su estructura de rutas fueron **propuestos**, no aprobados como contratos físicos definitivos. La próxima implementación debe estudiar utilidades de publicación local existentes antes de fijar rutas/codec/atomicidad.

La publicación READY no escribe ni adelanta EFFECTIVE. El puntero/registro EFFECTIVE pertenece a Runtime Adoption, en incremento posterior. No confundir un puntero a versión publicada con un commit de adopción.

## Consumidores y proyecciones posteriores

Engine y Delivery consumen los artefactos locales de una materialización **exacta** por `AlarmResolutionKey`. No requieren conexión a Cosmos para cargar esos contratos. Que una publicación Web Live pueda necesitar una salida operacional adicional corresponde a otro incremento; no convertir la discusión sobre *consumo de configuración* en una prohibición no comprobada sobre todas las integraciones posteriores de Delivery.

Live representa estado operacional actual y combina la configuración Delivery de la revisión efectiva con el estado producido por Engine. Management Projection representa historial de acciones, en otro job. Web no re-evalúa reglas ni re-resuelve B.2.
