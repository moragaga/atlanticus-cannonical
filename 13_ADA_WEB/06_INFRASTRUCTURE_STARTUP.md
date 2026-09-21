# ADA Web — Infrastructure Startup

Estado: **CURRENT CONTRACT / INTEGRATION PENDING**

## Invariante

ADA Generic debe poder levantar su composición base aunque:

```text
no exista Tool Source current
no exista Tool Projection
no existan KPI
no exista Latest
no exista Timeseries
Blob esté temporalmente indisponible
Cosmos esté temporalmente indisponible
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

Infraestructura disponible:

```text
AdaStorageNamespace
ToolPersistenceSettings
ToolPersistenceComposition
```

Resolver runtime:

```text
resolve_active_tool_projection()
```

Estados:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

El resolver no consulta Source para leer Projection activa.

## Lazy provider composition

`compose_tool_persistence()` construye stores/servicios sin ejecutar:

```text
Storage health check
Cosmos health check
Source read
Projection read
```

La conexión ocurre cuando la operación realmente lee/escribe.

Esto permite que la creación de composición no dependa por sí sola de disponibilidad de red.

## Estado de integración

ADA Generic todavía no consume esta capability desde su startup real.

CURRENT todavía contiene:

```text
_StartupToolProjectionStore
resolve_current_tool_projection
```

con `RuntimeError` cuando Source current no existe.

Por tanto:

```text
APPLICATION EMPTY/DEGRADED STARTUP
DECIDED / INFRASTRUCTURE READY / APP WIRING NOT YET IMPLEMENTED
```

## Collector lifecycle

Collector no debe poll durante Web composition.

```text
/health/*
/assets/*
/.auth/*
→ no start collector poller
```

Primer request real inicia el polling worker-local cuando Collector esté adjunto.

Browser requests no leen Cosmos inline.

## Degraded behavior

Tool:

```text
UNCONFIGURED
→ no Tool materialized
→ Web base remains available

UNAVAILABLE
→ capability unavailable
→ Web base remains available

INVALID
→ capability invalid + diagnostic
→ no silent fallback
```

Collector/data:

```text
missing KPI document
→ no forced application startup failure

source failure
→ request remains available
→ preserve last good cache where applicable
```

## Next

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

Debe implementar esta regla en el composition/runtime root real sin inventar una aplicación
paralela.
