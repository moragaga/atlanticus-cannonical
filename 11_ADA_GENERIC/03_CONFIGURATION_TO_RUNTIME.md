# ADA Generic — Configuration to Runtime

Estado: **CURRENT / STAGE 1 CLOSED**

## Configuration chain

```text
Tool Source
    ↓ exact ProjectionTarget during materialization workflow
Tool Projection durable
```

Tool Projection dispone de:

```text
LocalToolProjectionStore
CosmosToolProjectionStore
```

Providers:

```text
Source      local | blob
Projection  local | cosmos
```

## Runtime Tool read

CURRENT / FROZEN:

```text
runtime
→ resolve_active_tool_projection()
→ durable Tool Projection
```

No requiere Source disponible cuando ya existe Projection válida.

Materialization workflow separado:

```text
project_current_tool_source()
→ Source current
→ exact target
→ durable Tool Projection
```

No mezclar runtime read y materialization.

## Regla maestra

```text
CONFIGURATION DETERMINES EXISTENCE / STRUCTURE
DATA DETERMINES RUNTIME STATE
PERSISTED BUSINESS DATA DOES NOT DETERMINE WEB PROCESS EXISTENCE
```

## Tool resolution states

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

Estos estados no se transforman automáticamente en caída global de Web.

## Operational data chain CURRENT

Con Tool `READY` y Collector configurado:

```text
ToolStructure
    ↓
KPI Latest Delivery
KPI Timeseries Delivery
    ↓
AdaKpiCollector
    ↓
process cache
    ↓
Component KPI browser stores
```

Collector defaults:

```text
Latest poll       10 s
Timeseries poll  120 s
Browser refresh   10 s
```

Latest tiene prioridad cuando ambos reads están due.

One logical/browser KPI store per ToolComponent.

Subcomponents no crean stores.

## Render boundary

Estructura:

```text
ToolStructure
→ OperationalRenderBinding
```

Datos:

```text
Collector
→ dcc.Store / ToolComponent
```

Son fronteras distintas.

`OperationalRenderBinding` no transporta KPI state.

## Handoff al desarrollador

Stage 1 termina en:

```text
ToolStructure
+
existing browser stores
→ developer-owned concrete visualization
```

ADA Generic no impone un layout/body universal.

No requiere un incremento adicional de store-to-render wiring para declarar cerrada la entrega
genérica de datos.

## Estado

```text
ADA-GENERIC-STAGE-1
CLOSED / VERIFIED / CURRENT
```
