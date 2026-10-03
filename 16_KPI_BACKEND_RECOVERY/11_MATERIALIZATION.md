# KPI Backend Recovery — KPI Materialization

Estado: **CLOSED / VERIFIED / CURRENT**

## Purpose

KPI Materialization localizes the durable KPI Registry per Tool so backend consumers do not read Registry configuration directly from Cosmos during normal execution.

It does not merge all Tools into one global Registry.

It does not persist resolved secrets.

## Connections

Input:

```text
config/connections.json
```

Each key is a real `tool_key` resolving a named Cosmos connection.

Materialization reads connections at process startup.

## Local authority

```text
<VOLUMEN_PATH>/ada-kpi-engine/materialization/registries/<tool_key>.json
```

The local document preserves the full Registry and carries `tool_key`.

No resolved `CosmosSettings` or secrets are persisted.

## Registry binding

Per KPI:

```text
kpi_key
destination_keys
latest_enabled
series_enabled
series_hours
```

Invariant:

```text
series_enabled = true  → series_hours in 1..24
series_enabled = false → series_hours = None
```

## Update semantics

Per Tool:

```text
read durable Registry
validate
materialize full document
compare with local
atomic replace only when changed
```

Tool failure:

```text
preserve last-known-good
continue remaining Tools
raise iteration error after real failures
```

Removed Tool:

```text
remove corresponding local Registry
```

## Readiness

Missing remote Registry:

```text
KpiMaterializationRegistryPending
→ no process failure
→ retry after 30 s
```

Real acquisition/contract/storage error remains a failure.

## Consumers CURRENT

```text
Latest Delivery
Timeseries Delivery
```

Both consumers:

```text
wait for exact Tool-set readiness
read materialized Registries
validate
freeze process-lifetime configuration
do not hot-reload
```

Timeseries additionally consolidates all frozen Tool requirements into:

```text
required_columns
max_window_hours
```

## Qualification

Historical focused qualification remains:

```text
kpi-materialization-runtime   14 passed
Ruff / format                 PASS
```

No Materialization production change was required by the final History/Timeseries boundary correction.
