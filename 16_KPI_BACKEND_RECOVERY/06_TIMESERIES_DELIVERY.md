# KPI Backend Recovery — Timeseries Delivery

Estado: **CLOSED / VERIFIED / CURRENT**

## Implementation cut

```text
moragaga/atlanticus@2505196019fcc51e5f97ff66a3159beb87fe71f0
```

The legacy single-Tool/direct-history design is SUPERSEDED.

## CURRENT startup

Timeseries composition receives named Cosmos connections.

START/readiness:

```text
config/connections.json
→ named Tool CosmosSettings
→ expected tool_key set

LocalKpiRegistryStore
→ require exact convergence with expected Tool set
→ read each materialized Registry
→ validate and freeze configuration per Tool
→ build one consolidated read plan
```

If materialized Registries are not yet ready:

```text
status = materialization_pending
retry  = 30 s
```

Invalid Registry/config is a configuration error, not infinite readiness.

Configuration freezes after first successful materialization.

No hot reload during process lifetime.

## Consolidated read plan

```text
required_columns = sorted union of series_enabled KPI keys
max_window_hours = max(series_hours)
```

Constraints:

```text
0 <= max_window_hours <= 24
series_enabled=true requires series_hours
```

The rolling is read once per pending watermark/read-plan window, not once per Tool.

## Logical time contract

```text
TIMESERIES_STEP_SECONDS = 120
```

`output_end` is the absolute epoch floor of `HistorianAuthority.watermark_utc` to 120 seconds.

Each series window is:

```text
(output_end - series_hours, output_end]
```

Values are exact-grid only.

No:

```text
interpolation
nearest
aggregation
forward fill
backfill
```

Missing logical timestamps hydrate to `null`.

Requested KPI without physical rolling series produces a logical null series.

## Historian coherence

Timeseries reads:

```text
HistorianAuthority
+
rolling current.parquet
```

Operational data access uses:

```text
DatasetRuntime.read_schema
DatasetRuntime.scan_table
```

Timeseries rolling repository does not own direct Parquet I/O.

Coherence guards:

```text
rolling metadata watermark == HistorianAuthority watermark
rolling historian_revision == HistorianAuthority revision
metadata/schema token unchanged across schema-read and scan
```

Missing publication is explicit.

Invalid/corrupt rolling is explicit.

## Shared KPI dataset boundary

Timeseries consumes:

```text
ada.kpis.history.dataset
```

for:

```text
rolling DatasetDefinition / target
metadata decode
projection validation
schema token comparison
```

Timeseries process does not import PyArrow directly.

## Per-Tool checkpoint

State key:

```text
namespace = ('kpi-timeseries-delivery', 'tools', <tool_key>)
name      = checkpoint
```

Payload:

```text
watermark_utc
registry_revision
registry_digest
```

Watermark is the aligned 120 s output end that was successfully published/unchanged and checkpointed.

Invariants:

```text
checkpoint watermark must not regress
same registry_revision + different registry_digest = configuration error
Tool progress is independent
```

No global legacy checkpoint shim.

## Downtime behavior

Timeseries coalesces to the latest coherent output end.

It does not replay intermediate historical output snapshots after downtime.

## Publication

Each Tool owns a Cosmos connection and repository.

Container:

```text
ada-kpi-timeseries-delivery
partition key = /partition_id
TTL           = None
```

Document identity:

```text
id            = timeseries
partition_id  = kpis
document_type = ada_kpi_timeseries_delivery
```

Output:

```text
schema_version = 2
step_seconds   = 120
```

Manifest includes:

```text
revision
configuration_revision
tool_projection_revision
historian_revision
published_at_utc
```

Revision payload includes Historian/config/tool/data identity and excludes `published_at_utc`.

## Parallelism

Publication uses bounded `ThreadPoolExecutor`.

Workers own only Tool snapshot publication.

Main thread retains:

```text
lease validation
cancellation
fenced checkpoint commit
runtime-context mutation
failure aggregation
```

Successful Tools commit checkpoints even if another Tool fails.

After processing all results, any failures raise one aggregate iteration error.

Idempotent retry:

```text
publish succeeds
checkpoint fails
→ next iteration republishes
→ matching revision resolves UNCHANGED
→ checkpoint can advance
```

## Lifecycle

`--run-once` behavior remains one runtime iteration.

Materialization readiness does not block bootstrap indefinitely:

```text
materialization_pending
→ next delay 30 s for resident process
→ run-once exits after that iteration
```

## Qualification

Final focused qualification:

```text
processes/kpi-timeseries-delivery
28 passed

Ruff check
PASS

Ruff format --check
PASS

git diff --check
PASS
```

## UNVERIFIED / BLOCKED

```text
real multi-Tool Cosmos run
full Runtime -> Historian -> Timeseries E2E
restart/recovery through deployed application
production Azure credentials/network behavior
RU/load/performance profile
```

These validations are BLOCKED by required Web corrections and do not reopen the CURRENT Timeseries contract.
