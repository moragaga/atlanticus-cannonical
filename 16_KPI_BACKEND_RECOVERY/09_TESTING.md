# KPI Backend Recovery — Testing

Estado: **VERIFIED LOCALLY / OPERATIONAL E2E BLOCKED**

## Final focused qualification

Reported after integration on `main`:

```text
kpis/history
31 passed

processes/kpi-historian
45 passed

processes/kpi-timeseries-delivery
28 passed

Ruff check
PASS

Ruff format --check
PASS

git diff --check
PASS
```

## Qualification execution detail

Running Historian and Timeseries test directories together in one pytest process caused a collection collision because both packages expose a top-level:

```text
tests.support
```

Observed failure:

```text
Timeseries tests resolved tests.support from Historian tests
```

Each package suite was then executed in its own pytest process and passed.

Classification:

```text
production regression = NO
package-focused qualification = VERIFIED
combined test namespace collision = test harness limitation
```

No production change was made to work around this collection behavior.

## What is VERIFIED

```text
History logical contract
History reusable dataset representation
PyArrow isolation to ada.kpis.history.dataset
Historian process without direct PyArrow ownership
Historian Runtime-owned dataset I/O
durable history + error history
rolling metadata/schema/grid/horizon
rolling incremental update
rolling durable rebuild
rolling type transitions
rolling empty coverage
HistorianAuthority ordering/coherence

Timeseries materialized Registry readiness
Timeseries frozen per-Tool configuration
consolidated read plan
DatasetRuntime rolling reads
Authority/rolling coherence
120 s logical alignment
null hydration behavior
schema_version 2 output
per-Tool checkpoint
checkpoint regression guard
registry digest integrity guard
bounded parallel publication
partial failure independent progress
idempotent unchanged publication
```

## Previous qualification retained

```text
kpi-materialization-runtime
14 passed

kpi-delivery-runtime
28 passed

kpi-connections
7 passed
```

Those suites were not reopened by the final History/Timeseries boundary correction.

## Testing policy

Test:

```text
behavior
contracts
invariants
regressions
failure ordering
recovery
public integration surfaces
```

Do not create tests solely to freeze internal implementation shape or visual CSS/markup structure.

## BLOCKED / UNVERIFIED

```text
full KPI operational E2E
real multi-Tool Cosmos delivery
production Azure credentials/network behavior
runtime restart/readback through deployed stack
RU/load/performance profile
```

Reason for E2E block:

```text
required Web corrections must be completed before the complete application can be configured and exercised
```
