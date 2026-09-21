# ADA Web — Infrastructure Startup

Estado: **CLOSED / VERIFIED / CURRENT**

## Invariante

ADA Generic puede levantar su composición base aunque:

```text
no exista Tool Source current
no exista Tool Projection
no existan KPI
no exista Latest
no exista Timeseries
una capability externa esté temporalmente indisponible
```

La disponibilidad de una capability no define la existencia del proceso Web.

## Separación

```text
APPLICATION EXISTENCE
!= TOOL CONFIGURATION EXISTENCE
!= EXTERNAL INFRASTRUCTURE AVAILABILITY
!= BUSINESS DATA AVAILABILITY
```

## Tool persistence CURRENT

```text
AdaStorageNamespace
ToolPersistenceSettings
ToolPersistenceComposition
resolve_active_tool_projection()
```

Estados:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

Runtime activo no consulta Source para leer Projection durable.

## Bootstrap CURRENT

ADA Generic consume la capability desde su startup real:

```text
AdaGenericSettings
→ provider/client settings
→ ToolPersistenceComposition
→ resolve_operational_tool_projection()
→ durable Tool Projection
→ WebApplicationDefinition
```

La ruta anterior basada en:

```text
_StartupToolProjectionStore
resolve_current_tool_projection
```

fue removida.

No existe fallback legacy.

## Provider lifecycle

Los clientes utilizados sólo para resolver Tool tienen lifecycle corto y se cierran después de la
operación de bootstrap.

KPI Delivery usa su propia configuración Cosmos cuando Collector está habilitado.

No asumir conexión compartida con Tool Projection.

## Collector lifecycle

Collector no hace polling durante Web composition.

```text
/health/*
/assets/*
/.auth/*
→ no start collector poller
```

El primer request de aplicación elegible inicia polling worker-local.

Browser callbacks consumen cache de proceso, no Cosmos inline.

## Degraded behavior

Tool:

```text
UNCONFIGURED
→ Web base remains available

UNAVAILABLE
→ Web base remains available + diagnostic

INVALID
→ Web base remains available + invalid diagnostic
→ no silent fallback
```

Collector/data:

```text
KPI Delivery not configured
→ Web remains available without Collector

missing KPI document
→ no application startup failure

read failure after valid cache
→ preserve last good cache where contract allows
```

## Render/data boundary

Startup no construye una visualización Tool-specific obligatoria.

ADA Generic entrega:

```text
ToolStructure
KPI browser stores
```

La aplicación/desarrollador concreto decide su representación.

## Estado

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
CLOSED / VERIFIED / CURRENT

ADA-GENERIC-COLLECTOR-RUNTIME-WIRING
CLOSED / VERIFIED / CURRENT

ADA-GENERIC-STAGE-1
CLOSED / VERIFIED / CURRENT
```
