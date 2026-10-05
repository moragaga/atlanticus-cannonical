# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global — FROZEN

```text
uv; no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
one focus per increment
Git read-only unless explicit authorization
```

## Operational Data consumer contract — FROZEN / CLOSED

Único contrato CURRENT:

```text
DataInputSpec
DataView
DataInputContext
DataInputPlanner
DataInputLoadPlan
DataInputLoader
LoadedDataInputs
DataViewBinding
```

Pipeline:

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

Principio congelado:

```text
optimization by source/view
consumption by input identity
```

## Operational Data legacy — SUPERSEDED / REMOVED

Quedan retirados:

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

No reintroducirlos para compatibilidad.

`partition_dimensions` sigue siendo válido exclusivamente como layout físico de Dataset/materialization.

## KPI — CURRENT / CLOSED

`KpiSpec` declara `inputs: tuple[DataInputSpec, ...]`.

Resolvers consumen `DataInputContext` por `input_key`.

KPI Runtime usa `DataInputPlanner` y `DataInputLoader`.

No existe adapter hacia el contrato retirado.

## Alarm Runtime — BLOCKED by explicit prioritization

Decisión anterior implícita:

```text
mantener pipeline legacy temporalmente porque Alarm Runtime lo consume
```

queda **SUPERSEDED**.

Decisión CURRENT:

```text
priorizar contrato final y distribución
aceptar Alarm Runtime roto temporalmente
migrar Alarm después a un contrato compatible con DataInputSpec/DataInputContext
no restaurar legacy
```

Esta decisión no supersede el dominio Alarm, lifecycle, persistence, modeler ni delivery; sólo su integración de Operational Data.

## Distribution — NEXT

Siguiente foco único:

```text
artifact generation
artifact qualification
.env.detail exhaustive audit
distribution regeneration
isolated consumer qualification
```

No mezclar Alarm Runtime en ese incremento.
