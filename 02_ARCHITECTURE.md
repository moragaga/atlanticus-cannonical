# Atlanticus — Architecture

Estado: **CURRENT — SINGLE OPERATIONAL DATA INPUT CONTRACT**

## Regla principal

Atlanticus es modular y reusable. ADA y Command Center son consumidores.

## Operational Data — FROZEN

El único contrato de consumo CURRENT es:

```text
DataInputSpec
    input_key
    source
    view
    columns
    selection
```

Principio:

```text
optimization by source/view
consumption by input identity
```

El planner consolida lecturas físicas por:

```text
(source, view)
```

El consumidor recibe frames por:

```text
input_key
```

Esto permite que un mismo consumidor solicite la misma fuente/vista varias veces con selectores diferentes sin perder identidad lógica.

## Operational Data pipeline — FROZEN

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

No existe camino legacy paralelo.

## Logical vs physical boundary — FROZEN

```text
DataView
    logical consumer view

DataViewBinding
    binding source/view -> materialization + technical loading metadata

Dataset materialization partition_dimensions
    physical storage layout
```

No introducir nuevamente `DataPartition` como contrato de consumo ni filtrar layout físico hacia definiciones de consumidores.

## Consumer rule

Consumidores genéricos como KPI dependen del contrato neutral `operational-data-core`.

Builders de fuentes pueden producir `DataInputSpec`, pero el contrato de dominio del consumidor no debe depender de loaders, pandas, pyarrow ni clientes físicos.

## KPI — CURRENT

```text
KpiSpec.inputs -> tuple[DataInputSpec, ...]
KpiResolver    -> Callable[[DataInputContext], object]
```

Los modos simples requieren un input lógico; CUSTOM puede consumir varios inputs por `input_key`.

## Alarm — BLOCKED integration boundary

El dominio Alarm permanece válido.

Alarm Runtime todavía usa el contrato retirado y por eso está BLOCKED hasta una migración explícita al nuevo contrato. No restaurar legacy para mantenerlo funcionando.

## Generic Web capabilities

```text
Source Core / Local / Blob
Projection Core
Storage Namespace
Storage Topology
Users
Profiles
Navigation
Manager
Master Projection
```

## Storage Namespace CURRENT

```text
StorageNamespace(application_namespace, scope_namespace)

application_prefix = <application_namespace>
scope_prefix       = <application_namespace>/<scope_namespace>
```

## Clean cutover rule

```text
contracts before consumers
clean replacement
no legacy aliases
no dual write
no compatibility storage path
```
