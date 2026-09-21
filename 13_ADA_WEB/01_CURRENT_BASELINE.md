# ADA Web — Current Baseline

Estado: **CURRENT**

## Implementación auditada

```text
moragaga/atlanticus@bc8eafc21a65e3f9aff044c232e2562cd490c49f
```

## KPI Registry

```text
CLOSED / VERIFIED / CURRENT
```

## KPI Definition

```text
CLOSED / VERIFIED / CURRENT
```

## KPI Collector

```text
scopes/ada/web/kpis/collector
ada-web-kpi-collector==0.1.0
```

Public capability incluye:

```text
AdaKpiCollector
CosmosKpiDeliveryReader
AdaKpiCollectorPollingRuntime
AdaKpiCollectorWebIntegration
attach_ada_kpi_collector
component_kpi_store_id
resolve_kpi_collector_browser_update
```

Defaults:

```text
Latest polling      10 s
Timeseries polling 120 s
Browser refresh     10 s
```

One Component Store per ToolComponent.

Subcomponent no posee store propio.

## ADA Generic operational bootstrap

CURRENT:

```text
AdaGenericSettings
→ ToolPersistenceComposition
→ durable Tool Projection resolution
→ WebApplicationDefinition
```

Tool resolution:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

Estados degradados preservan Web base según contrato.

## Collector runtime wiring

Con Tool `READY` y KPI Delivery configurado:

```text
ToolStructure
→ AdaKpiCollector
→ attach_ada_kpi_collector
→ Web runtime
```

Tool Projection y KPI Delivery usan settings/conexiones independientes.

Collector no realiza polling durante composition.

## Operational Render boundary

CURRENT:

```text
OperationalRenderBinding
→ structure only
```

No incluye `ComponentStoreSnapshot`.

Collector no depende del package `ada-web-operational-render-binding`.

## Generic data delivery boundary

```text
Cosmos KPI Delivery
→ Collector
→ worker cache
→ dcc.Store / ToolComponent
→ developer
```

ADA Generic no es owner del body específico de una Tool.

## Qualification de cierre

Observado antes del checkpoint final:

```text
operational-render-binding  7 passed
kpis/collector              56 passed
ada-generic-application     86 passed

TOTAL                       149 passed

ruff check                  PASS
ruff format --check         PASS
git diff --check            PASS
```

## Python

Project baseline:

```text
3.14.7
```

Se mantiene el open item preexistente de metadata en packages que todavía declaren:

```text
requires-python ==3.14.2
```

Clasificación:

```text
PYTHON-METADATA-ALIGNMENT
OPEN / SEPARATE
```

No pertenece a ADA Generic Stage 1.

## Estado

```text
ADA-GENERIC-STAGE-1
CLOSED / VERIFIED / CURRENT
```
