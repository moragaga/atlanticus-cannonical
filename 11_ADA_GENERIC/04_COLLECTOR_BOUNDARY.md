# ADA Generic — Collector Boundary

Estado: **CLOSED / VERIFIED / CURRENT**

## Implementación CURRENT

```text
scopes/ada/web/kpis/collector
ada-web-kpi-collector==0.1.0
```

Checkpoint:

```text
moragaga/atlanticus@d484569cbe0290f38f239481cde81b13a23deecf
```

## Semántica congelada

Component es la frontera funcional de datos de la Tool.

```text
1 ToolComponent = 1 logical KPI Store
0..N Subcomponents per Component
Subcomponent != Store
Subcomponent != Collector
```

System KPI destinations como `global_indicators` y `time_status` no se convierten implícitamente
en Component Stores.

## Inputs

```text
Latest Delivery Cosmos
ada-kpi-latest-delivery
id=latest
partition_id=kpis
schema=1

Timeseries Delivery Cosmos
ada-kpi-timeseries-delivery
id=timeseries
partition_id=kpis
schema=2

ToolStructure CURRENT
Tool projection revision CURRENT
```

Reader CURRENT:

```text
CosmosKpiDeliveryReader
```

## Scheduling CURRENT

```text
Latest interval default       10 s
Timeseries interval default  120 s
Browser refresh default       10 s
```

Cuando ambos reads están due, Latest se ejecuta primero.

## Cache / worker lifecycle

```text
one collector/cache per worker
one polling thread per worker PID
first real request starts thread
health/assets/auth infrastructure requests do not start thread
request path never reads Cosmos inline
```

## Latest / Timeseries coherence

No se exige atomicidad cross-document.

Server cache compatibility:

```text
(configuration_revision, tool_projection_revision)
```

Rules:

```text
new compatible Latest
→ publish immediately
→ keep compatible Timeseries

new Latest with changed compatibility
→ publish Latest
→ drop incompatible Timeseries

incompatible Timeseries
→ ignore
→ never displace current Latest

stale Latest watermark
→ reject

stale Timeseries end
→ reject

missing document
→ keep last good

invalid source/contract
→ keep state unchanged
```

## Browser stores

ID:

```text
{
  type: ada-kpi-component-store,
  tool: tool_key,
  component: component_key
}
```

Shape conceptual:

```text
tool_key
component_key
latest
Timeseries
```

Browser callback:

```text
reads process snapshot only
never calls refresh_latest
never calls refresh_timeseries
never calls Cosmos
```

El merge browser protege contra workers desfasados usando marcadores monotónicos independientes
para Latest y Timeseries.

## Web composition

Attachment CURRENT:

```text
attach_ada_kpi_collector(WebApplicationDefinition, collector)
```

Agrega:

```text
ada-kpi-collector WebModule
browser Interval
browser revision Store
one Component Store per ToolComponent
cache-only callback
```

Duplicate attachment se rechaza.

## Web Observability

Dependency:

```text
WEB_OBSERVABILITY_SERVICE_KEY
```

Incident policy:

```text
KpiDeliveryReadError       -> WARNING deduplicado
KpiCollectorContractError -> ERROR deduplicado
other refresh error       -> ERROR deduplicado
polling runtime failure   -> CRITICAL
recovery                  -> clear source incident
```

## Qualification

```text
scripts/scopes/ada/check.sh kpi-collector
53 passed
commented mirrors PASS

real create_web_application smoke
PASS

scripts/scopes/ada/check.sh application
63 passed
```

## OPEN después del cierre

Sólo queda wiring operacional:

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

El siguiente chat debe **integrar este collector**, no diseñar otro.

Debe inspeccionar la composición operacional existente y resolver allí Tool + revision + Cosmos
antes de instanciar `AdaKpiCollector` y aplicar `attach_ada_kpi_collector`.

## No reabrir

```text
polling intervals
one-store-per-component
Latest priority
server compatibility rule
browser merge semantics
WebObservability service
attachment contract
```

salvo conflicto demostrado por implementación CURRENT.
