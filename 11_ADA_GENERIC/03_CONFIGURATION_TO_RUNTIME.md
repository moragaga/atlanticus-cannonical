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
ADA Generic Collector / UI stores
```

Los últimos dos pasos de Collector/UI stores son la siguiente frontera de implementación; los outputs KPI ya están materializados y CURRENT.

## Regla maestra

```text
CONFIGURATION DETERMINES EXISTENCE
DATA DETERMINES STATE
```

La UI no debe depender de la primera medición para crear estructura.

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

## Handoff al Collector

Collector debe partir de estas superficies y del contrato Tool CURRENT.

Decidido:

```text
Latest read is priority.
Latest and Timeseries use different load intervals.
```

No decidido todavía:

```text
exact interval values
read synchronization/coherency policy
exact UI store wiring
```
