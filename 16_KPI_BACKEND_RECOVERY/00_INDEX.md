# KPI Backend Recovery / Materialization / Delivery — Index

Estado: **CURRENT — KPI RUNTIME DATA-INPUT MIGRATION CLOSED / VERIFIED LOCALLY**

## Autoridad de este cierre

```text
Implementation
moragaga/atlanticus@777f3a0894a58f7275473ab34ce6b33cf767f9e7

Canonical inspected before replacement
moragaga/atlanticus-cannonical@44d3c803f60d1a1630d3a3374a663447cfe21248

Historical decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
relevant binary artifacts located; substantive compatibility UNVERIFIED
```

Git permanece **SOLO LECTURA**.

## Current delta

KPI Runtime dejó de consumir el Operational Data legacy contract.

CURRENT:

```text
KpiSpec.inputs
    ↓
DataInputSpec
    ↓
DataInputPlanner
    ↓
DataInputLoader
    ↓
DataInputContext
```

No existe adapter hacia `DataRequirement`.

## Qualification

```text
kpis/core/tests                  29 passed
kpis/evaluation/tests            21 passed
processes/kpi-runtime/tests      44 passed
Ruff                             PASS
format                           PASS
```

## Other KPI capabilities

Latest Delivery, Historian, Timeseries Delivery, materialization and named connections retain their previously closed/current status. They were not redesigned in this hito.

## Remaining boundary

```text
full distributed/application operational E2E = UNVERIFIED
distribution/tooling qualification = NEXT
Alarm Runtime migration = separate
```
