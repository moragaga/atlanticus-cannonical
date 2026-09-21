# ADA Generic — Configuration to Runtime

Estado: **CURRENT**

## Cadena CURRENT

```text
Tool Source/Projection
    ↓ exact ProjectionTarget
KPI Registry Source/Projection
    ↓ exact ProjectionTarget
KPI Definition Source/Projection

Operational data
    ↓
KPI Runtime durable evaluations
    ↓
KPI Historian materialization
    ↓
Latest Delivery Cosmos + Timeseries Delivery Cosmos
    ↓
AdaKpiCollector process cache
    ↓
Component KPI browser stores
```

La capability Collector y su Web attachment ya están implementados y calificados.

## Regla maestra

```text
CONFIGURATION DETERMINES EXISTENCE
DATA DETERMINES STATE
```

La estructura de stores existe desde `ToolStructure`; no depende de recibir primero una medición.

## Tool / KPI contracts

CURRENT:

```text
Tool ProjectionTarget
→ KPI Registry ProjectionTarget
→ KPI Definition ProjectionTarget
```

No reintroducir:

```text
KpiConfiguration legacy domain
private revision identities
expected_source_revision
legacy compatibility readers
```

## Backend outputs CURRENT

Latest:

```text
container = ada-kpi-latest-delivery
id = latest
partition_id = kpis
document_type = ada_kpi_latest_delivery
schema_version = 1
```

Timeseries:

```text
container = ada-kpi-timeseries-delivery
id = timeseries
partition_id = kpis
document_type = ada_kpi_timeseries_delivery
schema_version = 2
step_seconds = 120
```

## Collector CURRENT

```text
Latest poll      10 s default
Timeseries poll 120 s default
Browser refresh  10 s default
```

Compatibility:

```text
configuration_revision + tool_projection_revision
```

One logical KPI store per Tool Component; Subcomponents do not create stores.

## Siguiente handoff

Lo que falta no es otro contrato de Collector. Falta materializar esta última composición en la
aplicación operacional real:

```text
ToolConfiguration CURRENT
+ tool projection revision
+ Cosmos client/configuration CURRENT
    ↓
CosmosKpiDeliveryReader
    ↓
AdaKpiCollector
    ↓
attach_ada_kpi_collector(existing application definition)
```

Clasificación:

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```
