# KPI Backend Recovery — Configuration

Estado: **CURRENT for Materialization + Latest + Timeseries**

## Named connections

Package:

```text
ada-kpis-connections==1.0.0
```

Contract:

```text
config/connections.json
```

Each Tool entry references variable names:

```text
endpoint_var
database_var
credential_var
```

The document contains references, not resolved credentials.

Tool keys remain strict and invalid keys are not silently normalized.

## Materialized Registry location

```text
<VOLUMEN_PATH>/ada-kpi-engine/materialization/registries/<tool_key>.json
```

Consumers use the common materialization root, independent of their own `APPLICATION`.

## Materialization

```text
POLL_INTERVAL_SECONDS=30
```

Behavior:

```text
read named connections at startup
acquire Registry per Tool
write local full Registry
preserve LKG on per-Tool failure
remove local Registry for removed Tool
missing remote Registry => readiness pending / 30 s
real contract/connectivity error => failure
```

## Latest Delivery

Relevant settings:

```text
KPI_RUNTIME_APPLICATION
KPI_DELIVERY_MAX_WORKERS
poll interval default = 1 s
materialization readiness retry = 30 s
```

Configuration is frozen after materialized Registry readiness.

## Timeseries Delivery

CURRENT settings:

```text
KPI_HISTORIAN_APPLICATION
KPI_TIMESERIES_DELIVERY_POLL_INTERVAL_SECONDS
KPI_TIMESERIES_DELIVERY_MAX_WORKERS
APPLICATION
VOLUMEN_PATH
```

Defaults:

```text
poll interval = 1 s
max workers   = 2
```

Timeseries consumes the same named connections supplied by composition and the same materialized Registry set.

Startup readiness:

```text
expected Tool set must equal materialized Registry Tool set
missing/unexpected set => materialization_pending
retry = 30 s
```

Successful readiness freezes all per-Tool configurations for process lifetime.

No legacy single global Cosmos consumption connection remains in the CURRENT Timeseries path.

## Historian location

Timeseries identifies Historian state/storage through:

```text
KPI_HISTORIAN_APPLICATION
```

It reads:

```text
HistorianAuthority via AtomicStateStore
rolling dataset via DatasetRuntime over Historian application root
```

## Secrets

`config/connections.json` must not contain resolved secrets.

Resolved Cosmos credentials come from environment/deployment secret sources.

`.env.detail` exhaustive audit is a separate PLANNED front and was not reopened by this KPI closure.
