# Atlanticus — Current State

Estado: **CURRENT — OPERATIONAL DATA FINAL CONTRACT + KPI MIGRATION CLOSED / VERIFIED**

## Autoridad

```text
Implementation
moragaga/atlanticus@777f3a0894a58f7275473ab34ce6b33cf767f9e7

Canonical base before replacement
moragaga/atlanticus-cannonical@44d3c803f60d1a1630d3a3374a663447cfe21248
```

## CLOSED / VERIFIED en este hito

Operational Data quedó normalizado a un único contrato de consumo:

```text
DataInputSpec
    ↓
DataInputPlanner
    ↓
DataInputLoadPlan
    ↓
DataInputLoader
    ↓
LoadedDataInputs
    ↓
DataInputContext
```

El pipeline legacy fue removido de Operational Data:

```text
DataPartition
DataRequirement
DataSourceView
DataRuntimeContext
DataRequirementPlanner
DataLoadPlan
DataSourceViewLoadPlan
DataSourceLoader
LoadedDataSources
DataPartitionBinding
```

El registry físico usa directamente:

```text
DataView
    ↓
DataViewBinding
```

`partition_dimensions` permanece únicamente como concepto físico de Dataset/materialization.

## KPI CURRENT

KPI quedó migrado al contrato final.

`KpiSpec` declara:

```text
inputs: tuple[DataInputSpec, ...]
```

Los resolvers consumen por identidad local:

```text
context.get(input_key)
```

KPI Runtime consume:

```text
DataInputLoadPlan
DataInputLoader
```

No existe compatibilidad `DataInputSpec -> DataRequirement`.

## Alarm Runtime

Estado:

```text
BLOCKED / PLANNED MIGRATION
```

La implementación de Alarm Runtime todavía importa símbolos legacy removidos (`DataRequirement`, `DataLoadPlan`, `DataRequirementPlanner`, `DataRuntimeContext`).

Este quiebre es intencional y aceptado para priorizar el contrato final. No reintroducir aliases, adapters ni el pipeline retirado.

El dominio Alarm y sus contratos de lifecycle/persistence/modeler/delivery no quedan superseded por este cierre; el bloqueo es específicamente de su integración con Operational Data.

## Qualification observada

Operational Data:

```text
pytest core/tests planner/tests sources/tests   52 passed
ruff check                                     PASS
ruff format --check                            61 files formatted
legacy-symbol grep                             0 results
```

KPI:

```text
kpis/core/tests                                29 passed
kpis/evaluation/tests                          21 passed
processes/kpi-runtime/tests                    44 passed
ruff check                                     PASS
ruff format --check                            101 files formatted
```

La qualification es local reportada por el usuario; no equivale a CI ni Azure/Docker productivo.

## OPEN separado

```text
Alarm Runtime migration to the new data-input contract
artifact generation qualification
.env.detail exhaustive audit
distribution regeneration
isolated distributed consumer qualification
Python 3.14.7 / Trixie migration
production Azure / Entra qualification
```

## NEXT único

```text
ATLANTICUS-DISTRIBUTION-AND-TOOLING-FINAL-QUALIFICATION
```
