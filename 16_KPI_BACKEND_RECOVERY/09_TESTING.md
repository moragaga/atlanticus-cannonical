# KPI Backend Recovery — Testing

Estado: **VERIFIED LOCALLY — KPI DATA-INPUT MIGRATION QUALIFIED**

## Focused qualification 2026-10-05

```text
kpis/core/tests
29 passed

kpis/evaluation/tests
21 passed

processes/kpi-runtime/tests
44 passed

Ruff check
PASS

Ruff format --check
PASS
```

## Test execution note

These package suites are executed in separate pytest processes.

The workspace contains multiple top-level `tests` packages and combined invocation previously caused `tests.support` namespace collisions. This is a test-harness/package-layout limitation, not evidence of production regression.

## What is VERIFIED

```text
KpiSpec DataInputSpec contract
simple-mode input behavior
CUSTOM multi-input behavior
DataInputContext evaluation
source tracing from declared inputs
KPI Runtime DataInputPlanner composition
KPI Runtime DataInputLoader execution
Over KPI behavior preserved
reprocess behavior preserved
lint and format
```

## Operational Data dependency qualification

The underlying final Operational Data contract was also qualified separately:

```text
52 passed
Ruff PASS
format PASS
legacy-symbol grep = 0
```

## BLOCKED / UNVERIFIED

```text
full distributed KPI application E2E
artifact installability after current-head regeneration
real multi-Tool Cosmos behavior in final distribution
production Azure credentials/network behavior
performance/RU profile
```
