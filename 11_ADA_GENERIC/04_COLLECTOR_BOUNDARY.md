# ADA Generic — Collector Boundary

Estado: **CLOSED / VERIFIED / CURRENT**

## Implementación CURRENT

```text
scopes/ada/web/kpis/collector
ada-web-kpi-collector==0.1.0
```

## Semántica congelada

```text
1 ToolComponent = 1 logical KPI Store
0..N Subcomponents per Component
Subcomponent != Store
Subcomponent != Collector
```

## Inputs

```text
Latest Delivery
Timeseries Delivery
ToolStructure
Tool projection revision
```

Reader CURRENT:

```text
CosmosKpiDeliveryReader
```

## Scheduling

```text
Latest interval default       10 s
Timeseries interval default  120 s
Browser refresh default       10 s
```

Latest tiene prioridad cuando ambos reads están due.

## Cache / worker lifecycle

```text
one collector/cache per worker
one polling thread per worker PID
first real request starts thread
health/assets/auth requests do not start thread
browser request never reads Cosmos inline
```

## Coherence

No se exige atomicidad cross-document.

Compatibility:

```text
configuration_revision
tool_projection_revision
```

Missing/invalid data no reemplaza un último estado bueno válido.

## Web composition CURRENT

Attachment:

```text
attach_ada_kpi_collector(WebApplicationDefinition, collector)
```

ADA Generic puede existir sin Collector.

Con Tool `READY` y configuración KPI Delivery completa, el bootstrap adjunta Collector antes de
crear el runtime Web.

Tool Projection Cosmos y KPI Delivery Cosmos son conexiones conceptualmente separadas.

## Browser delivery boundary

Collector Web integration publica:

```text
1 dcc.Store por ToolComponent
```

El navegador consume cache de proceso.

No lee Cosmos inline.

## Render dependency removed

El contrato anterior:

```text
collector.operational_render_binding
OperationalComponentBinding.store
bind_operational_render(structure, stores)
```

fue eliminado.

CURRENT:

```text
AdaKpiCollector
does not depend on
ada-web-operational-render-binding
```

`OperationalRenderBinding` no transporta snapshots del Collector.

## Handoff

```text
Collector
→ browser dcc.Store
→ developer / concrete Tool visualization
```

La representación visual específica queda fuera del Collector y fuera del ownership genérico de
ADA Generic.

## No reabrir

```text
polling intervals
one-store-per-component
Subcomponent boundary
Latest priority
server compatibility
browser merge semantics
WebObservability service
attachment contract
render/data separation
```

salvo conflicto demostrado por implementación CURRENT.
