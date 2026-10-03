# KPI Backend Recovery — Source Ledger

Estado: **AUDIT LEDGER / CURRENT**

## Implementation cut

```text
moragaga/atlanticus@38bcd8c5607d67f999e2bc4bf9dbf176c8340588
date = 2026-10-03T01:09:03Z
```

Este commit contiene el cierre de Historian rolling.

El commit padre inmediato:

```text
c0c01cc816687fc7db20a3559f6ac9b46e6df7d4
```

corresponde a otro frente de tooling/Data Explorer y no forma parte del contrato Historian de este
hito.

## Decisions

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

No se observó una decisión frozen específica que describa el rolling Historian 24 h / 30 s o el
reemplazo Timeseries multi-Tool.

Clasificación:

```text
Historian rolling implementation   IMPLEMENTED + VALIDATED / CURRENT
specific frozen decision           NOT OBSERVED
decision formalization             OPEN DOCUMENTAL
```

No existe conflicto con una decisión frozen incompatible conocida.

## Canonical before replacement

```text
moragaga/atlanticus-cannonical@bac346a4c65e7a7d689e74a421656e75fe1d27b3
date = 2026-10-02T23:56:25Z
```

Ese canonical todavía clasificaba:

```text
Historian durable history     CURRENT
Historian rolling             PLANNED
Timeseries replacement        PLANNED after Historian
```

La promoción de Historian rolling a CURRENT es el delta documental de este cierre.

## Inspected CURRENT implementation

### Connections

```text
scopes/ada-kpi-engine/kpis/connections/
```

CURRENT:

```text
named Tool connections
dynamic environment variable declarations
strict tool_key
duplicate physical endpoint/database rejection
```

### Materialization

```text
scopes/ada-kpi-engine/kpis/materialization/
scopes/ada-kpi-engine/processes/kpi-materialization/
```

CURRENT:

```text
full Registry + root tool_key
per-Tool local JSON
sequential acquisition
LKG preservation on failures
30 s readiness retry for missing remote Registry
```

### Latest Delivery

```text
scopes/ada-kpi-engine/processes/kpi-delivery/
```

CURRENT:

```text
materialized Registry consumer
30 s startup readiness
process-lifetime freeze
1 s normal polling
parallel per-Tool publication
per-Tool checkpoints
partial failure isolation
```

### Historian

```text
scopes/ada-kpi-engine/kpis/history/
scopes/ada-kpi-engine/processes/kpi-historian/
```

CURRENT:

```text
daily durable long history
error history
HistorianAuthority
rolling wide current.parquet
24 h maximum physical horizon
30 s strict grid
UTC timestamp
observed physical coverage only
atomic replacement
incremental update from new batches
rebuild from durable history
shared rolling metadata contract
```

Exact rolling path:

```text
<application_root>/timeseries/current.parquet
```

Exact metadata key:

```text
ada_kpi_timeseries
```

Commit ordering:

```text
durable history
→ rolling
→ HistorianAuthority
```

### Timeseries Delivery

```text
scopes/ada-kpi-engine/processes/kpi-timeseries-delivery/
```

CURRENT legacy implementation:

```text
direct Registry Cosmos reader
single configuration
direct durable-history scan
global checkpoint
120 s grid
```

PLANNED / NEXT replacement:

```text
materialized Registry
named connections
HistorianAuthority
Historian rolling
logical hydration
per-Tool publication
per-Tool checkpoints
```

## Qualification evidence

Reported and completed after final Ruff formatting:

```text
kpis/history
27 passed
ruff check                 PASS
ruff format --check        PASS

processes/kpi-historian
47 passed
ruff check                 PASS
ruff format --check        PASS

git diff --check           PASS
```

## Epistemic status

### VERIFIED

```text
rolling implementation is present in atlanticus:main
rolling shared contract is public
rolling path/schema/metadata/coherence are implemented
incremental/recovery/atomic behavior has focused test coverage
final focused test and Ruff gates pass
Timeseries legacy implementation remains in main
```

### INFERRED

```text
No additional inference is required to promote Historian rolling to CURRENT.
```

### ASSUMED

```text
No unverified assumption was converted into a CURRENT Historian contract.
```

### PROPOSED

```text
Timeseries replacement behavior not yet frozen in implementation:
logical/output step
per-Tool checkpoint exact schema/state key
missed-watermark coalescing
output schema_version decision
```

### UNVERIFIED

```text
new Timeseries consumer
full operational Historian -> Timeseries E2E
real Azure/Cosmos production behavior
load/performance profile
```

## Conflict ledger

### Implementation vs canonical

Before this replacement:

```text
CONFLICT / STALE DOCUMENTATION
canonical said Historian rolling = PLANNED
main now implements and validates Historian rolling
```

This replacement resolves that stale status.

### Implementation vs decisions

```text
NO KNOWN FROZEN CONFLICT
```

There is no observed frozen decisions entry specific to the new rolling contract.

Formalizing that decision remains documentary work, not a blocker for recognizing implemented
reality.

### Python baseline

Current `kpi-historian/pyproject.toml` remains:

```text
requires-python = ==3.14.2
```

This hito did not modify the runtime baseline.

Any migration to Python 3.14.7 is a separate concern and must not be mixed into Timeseries Delivery
unless explicitly opened.
