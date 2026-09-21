# ADA Generic — Configuration to Runtime

Estado: **CURRENT / BOOTSTRAP GAP OPEN**

## Configuration chain CURRENT

```text
Tool Source
    ↓ exact ProjectionTarget
Tool Projection durable
    ↓
KPI Registry Projection
    ↓
KPI Definition Projection
```

Tool Projection dispone de:

```text
LocalToolProjectionStore
CosmosToolProjectionStore
```

y de composición de providers:

```text
Source     local | blob
Projection local | cosmos
```

## Runtime Tool read

Dirección CURRENT/FROZEN:

```text
runtime
→ resolve_active_tool_projection()
→ durable Tool Projection
```

No requiere Source disponible cuando ya existe Projection válida.

Workflow de materialización:

```text
project_current_tool_source()
→ Source current
→ exact target
→ durable Tool Projection
```

No mezclar ambas operaciones.

## Regla maestra

```text
CONFIGURATION DETERMINES EXISTENCE/STRUCTURE
DATA DETERMINES STATE
PERSISTED STATE DOES NOT DETERMINE WEB PROCESS EXISTENCE
```

## Tool resolution states

CURRENT:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

Semántica:

```text
READY
projection válida disponible

UNCONFIGURED
provider accesible pero no existe projection/configuración

UNAVAILABLE
infraestructura necesaria para esa operación no responde

INVALID
contrato/documento obtenido es inválido
```

Estos estados no deben transformarse automáticamente en caída global de la Web.

## Operational data chain

Cuando existe Tool READY:

```text
ToolStructure
    ↓
KPI Runtime durable evaluations
    ↓
KPI Historian
    ↓
Latest Delivery + Timeseries Delivery
    ↓
AdaKpiCollector process cache
    ↓
Component KPI browser stores
```

## Collector CURRENT

```text
Latest poll      10 s default
Timeseries poll 120 s default
Browser refresh  10 s default
```

One logical KPI store per Tool Component.

Subcomponents do not create stores.

## Siguiente handoff

Antes de conectar Collector al startup real debe cerrarse:

```text
environment/.env
→ provider/client settings
→ AdaStorageNamespace
→ ToolPersistenceComposition
→ resolve_active_tool_projection
→ ADA Generic runtime/composition
```

Clasificación:

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

Después:

```text
ADA-GENERIC-COLLECTOR-RUNTIME-WIRING
PLANNED / AFTER BOOTSTRAP
```

No rediseñar Collector.
