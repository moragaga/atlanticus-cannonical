# KPI Backend Recovery — KPI Runtime

Estado: **CLOSED / VERIFIED / CURRENT — FINAL OPERATIONAL DATA INPUT CONTRACT**

## Data input contract

KPI Runtime consumes exclusively:

```text
DataInputLoadPlan
DataInputLoader
LoadedDataInputs
DataInputContext
```

Composition builds:

```text
DataInputPlanner().plan({
    spec.key: spec.inputs
})
```

`KpiSpec` owns:

```text
inputs: tuple[DataInputSpec, ...]
```

Resolvers consume:

```text
context.get(input_key)
```

## Invariant

```text
optimization by source/view
consumption by input identity
```

A consumer may request the same source/view multiple times with distinct local keys/selectors.

## Legacy status

Removed from KPI:

```text
source
partition
source_requirements
DataRequirement
DataRequirementPlanner
DataLoadPlan
DataSourceLoader
DataRuntimeContext
```

No compatibility adapter exists.

## Reprocess contract retained

The previous `REPROCESS_CURRENT` semantics remain current. This migration did not reopen persistence/reprocess behavior.

## Qualification observed

```text
kpis/core/tests               29 passed
kpis/evaluation/tests         21 passed
processes/kpi-runtime/tests   44 passed
Ruff                          PASS
format                        PASS
```
