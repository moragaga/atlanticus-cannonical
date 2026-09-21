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

Missing/invalid data no debe reemplazar un último estado bueno válido.

## Web composition

Attachment CURRENT:

```text
attach_ada_kpi_collector(WebApplicationDefinition, collector)
```

Generic Application no depende obligatoriamente del Collector.

## Refinamiento del siguiente paso

El canonical anterior decía:

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

Ese orden quedó refinado.

Antes del attachment operacional ya se cerraron:

```text
AdaStorageNamespace
Tool Projection durable local/cosmos
ToolPersistenceComposition
Tool projection resilient resolution
```

Por tanto el próximo foco ya no es diseñar ni conectar Collector directamente desde Source.

Siguiente:

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

Luego, con Tool Projection `READY`:

```text
ADA-GENERIC-COLLECTOR-RUNTIME-WIRING
PLANNED / AFTER BOOTSTRAP
```

## No reabrir

```text
polling intervals
one-store-per-component
Latest priority
server compatibility
browser merge semantics
WebObservability service
attachment contract
```

salvo conflicto demostrado por implementación CURRENT.
