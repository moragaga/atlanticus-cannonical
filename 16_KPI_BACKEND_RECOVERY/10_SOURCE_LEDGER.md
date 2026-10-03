# KPI Backend Recovery — Source Ledger

Estado: **AUDIT LEDGER / CURRENT**

## Implementation cut

```text
moragaga/atlanticus@2505196019fcc51e5f97ff66a3159beb87fe71f0
date = 2026-10-03T05:42:35Z
```

Parent:

```text
2f9b65c3ba2646d519abfb0bb49e095d6819d185
```

The final commit touches only the KPI History/Historian/Timeseries boundary plus `uv.lock`.

## Canonical before replacement

```text
moragaga/atlanticus-cannonical@38404e61c69978183cd515be4ca40afed7ef59e8
date = 2026-10-03T05:40:40Z
```

Stale canonical state before this replacement:

```text
Timeseries multi-Tool delivery = PLANNED
Timeseries direct legacy history path = CURRENT
Historian atomic file replacement = process-owned
Materialization consumer set = Latest only
History tabular representation = mixed across contract/rolling_dataset
qualification counts = pre-final-boundary values
```

Implementation CURRENT resolves those discrepancies.

## Decisions

```text
NOT INSPECTED
```

The closure explicitly prohibited reading `atlanticus-decisions`.

Therefore:

```text
implementation-vs-decisions conflict status = UNVERIFIED
```

No statement of compatibility or incompatibility with decisions is made in this ledger.

## CURRENT implementation inspected

### KPI History

```text
scopes/ada-kpi-engine/kpis/history/
```

CURRENT:

```text
contract.py
    logical durable DatasetDefinitions / targets

rolling.py
    logical rolling metadata and invariants

dataset.py
    reusable KPI PyArrow schemas/conversion
    rolling DatasetDefinition / target
    durable projection decode
    rolling encode/decode/projection
```

PyArrow is intentionally isolated to `dataset.py`.

### Historian

```text
scopes/ada-kpi-engine/processes/kpi-historian/
```

CURRENT:

```text
daily durable history
error history
HistorianAuthority
rolling current.parquet
DatasetRuntime for durable and rolling I/O
no direct PyArrow import in process materializers
```

### Timeseries Delivery

```text
scopes/ada-kpi-engine/processes/kpi-timeseries-delivery/
```

CURRENT:

```text
materialized Registry per Tool
named connections
lazy/frozen readiness
consolidated rolling read plan
HistorianAuthority coherence
DatasetRuntime rolling access
120 s output grid
schema_version 2
per-Tool checkpoints
bounded parallel publication
partial-failure progress preservation
```

### Materialization

CURRENT consumers:

```text
Latest Delivery
Timeseries Delivery
```

## Qualification evidence

```text
kpis/history                         31 passed
processes/kpi-historian             45 passed
processes/kpi-timeseries-delivery   28 passed

Ruff                                PASS
format                              PASS
git diff --check                    PASS
```

## Epistemic status

### VERIFIED

```text
final KPI commit is present in main
History dataset.py owns reusable Arrow representation
Historian process consumes shared dataset helpers
Timeseries rolling repository consumes DatasetRuntime
Timeseries multi-Tool implementation is present
per-Tool checkpoint contract is implemented
120 s output contract is implemented
focused package suites are green
```

### INFERRED

```text
No additional architectural inference is required to classify this KPI backend increment CLOSED locally.
```

### ASSUMED

```text
None promoted to CURRENT.
```

### PROPOSED

```text
Next separate focus: inspect Command Center / Alarm backend boundary.
```

### UNVERIFIED

```text
full KPI operational E2E
real multi-Tool Cosmos behavior
production Azure behavior
performance/RU profile
implementation-vs-decisions compatibility
```

## Conflict ledger

### Implementation vs canonical before replacement

```text
CONFLICT / STALE DOCUMENTATION
```

Canonical still described Timeseries replacement as future and process-level rolling atomicity.

This replacement updates canonical to the current implementation.

### Implementation vs decisions

```text
UNVERIFIED
```

Decisions were intentionally not read.

### Python baseline

Relevant current packages remain:

```text
requires-python = ==3.14.2
```

Project target baseline remains 3.14.7.

Migration is separate and was not mixed into this hito.
