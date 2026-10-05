# KPI Backend Recovery — Source Ledger

Estado: **AUDIT LEDGER / CURRENT**

## Implementation cut

```text
moragaga/atlanticus@777f3a0894a58f7275473ab34ce6b33cf767f9e7
date = 2026-10-05T19:17:18Z
```

## Canonical before replacement

```text
moragaga/atlanticus-cannonical@44d3c803f60d1a1630d3a3374a663447cfe21248
date = 2026-10-03T21:46:02Z
```

## Historical decisions

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Relevant Operational Data/KPI historical artifacts were located in DOCX/XLSX form. Their content was not textually inspectable through the available connector during this closure.

Therefore:

```text
implementation-vs-decisions compatibility = UNVERIFIED
```

## Current implementation inspected

### Operational Data

CURRENT exports and implementation use:

```text
DataInputSpec
DataInputContext
DataInputPlanner
DataInputLoadPlan
DataInputLoader
LoadedDataInputs
DataView
DataViewBinding
```

Legacy consumer contracts are absent from Operational Data.

### KPI Core

CURRENT:

```text
KpiSpec.inputs: tuple[DataInputSpec, ...]
KpiResolver: Callable[[DataInputContext], object]
```

### KPI Runtime

CURRENT:

```text
DataInputLoadPlan
DataInputLoader
```

## Qualification evidence

```text
Operational Data                 52 passed
KPI Core                         29 passed
KPI Evaluation                   21 passed
KPI Runtime                      44 passed
Ruff                             PASS
format                           PASS
Operational Data legacy grep     0 results
```

## Conflict ledger

### Implementation vs canonical before replacement

```text
CONFLICT / STALE DOCUMENTATION
```

Canonical still described older focal states and did not record the final Operational Data cutover, KPI input migration, or intentional Alarm Runtime block.

These replacement files reconcile canonical with current implementation.

### Implementation vs historical decisions

```text
UNVERIFIED
```

No conflict is asserted without reading the binary decision artifacts.

### Python baseline

Relevant current workspaces still use Python 3.14.2 while Project target remains 3.14.7/Trixie.

This is an existing separate migration, not part of the current contract closure.
