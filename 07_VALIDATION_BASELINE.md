# Atlanticus — Validation Baseline

Estado: **CURRENT — OPERATIONAL DATA FINAL CONTRACT + KPI MIGRATION QUALIFIED LOCALLY 2026-10-05**

## Autoridad

```text
Implementation  moragaga/atlanticus@777f3a0894a58f7275473ab34ce6b33cf767f9e7
Canonical base  moragaga/atlanticus-cannonical@44d3c803f60d1a1630d3a3374a663447cfe21248
```

## Operational Data qualification

Reportada por el usuario:

```text
pytest core/tests planner/tests sources/tests
52 passed

ruff check core planner sources
PASS

ruff format --check core planner sources
61 files already formatted

legacy-symbol git grep
0 results
```

Legacy-symbol scan cubrió:

```text
DataPartition
DataRequirement
DataSourceView
DataRuntimeContext
DataRequirementPlanner
DataLoadPlan
DataSourceLoader
LoadedDataSources
DataPartitionBinding
```

## KPI qualification

Reportada por el usuario:

```text
kpis/core/tests
29 passed

kpis/evaluation/tests
21 passed

processes/kpi-runtime/tests
44 passed

ruff check
PASS

ruff format --check
101 files already formatted
```

## Acredita

```text
single Operational Data consumer contract
legacy contract removal from Operational Data
DataView -> DataViewBinding registry normalization
KPI contract migration to DataInputSpec
KPI evaluation through DataInputContext
KPI Runtime through DataInputPlanner/DataInputLoader
no compatibility adapter between new and removed contracts
```

## Alarm qualification status after cutover

```text
Alarm domain contracts: not reopened
Alarm previous E2E: historical evidence retained
alarms-runtime current executable integration: BLOCKED
```

Reason:

```text
Alarm Runtime still imports removed Operational Data legacy symbols.
```

This is intentional and must not be hidden with compatibility code.

## No acredita

```text
monorepo-wide pytest after intentional Alarm break
Alarm Runtime migration
CI
distributed artifacts
isolated installation/distribution
.env.detail completeness
Docker multi-process qualification
Azure / Entra production qualification
Python 3.14.7/Trixie migration
```
