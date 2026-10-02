# Source Storage — Open Contracts

Estado: **CURRENT — CORE CLOSED / NAMESPACE-COMPOSITION OPEN**

## Core Source — CLOSED / FROZEN

Frozen:

```text
SourceKey
SourceReleaseId
SourceReleaseRef
immutable releases
manifest commit point
basis_release
SourceStore
ConcurrencyToken
CAS/current promotion
History opaque cursor
exact reads
integrity
same-content republish may create a new release
```

## Projection handoff — CLOSED / FROZEN

```text
ProjectionTarget = SourceKey + SourceReleaseRef
project(target) does not reread current
exact provenance
retry same target
failure does not rollback Source
ProjectionStore.get_active/replace_active
```

## OPEN / NEXT — namespace and composition ownership

Implementation currently contains:

```text
scopes/ada/web/storage/namespace
    AdaStorageNamespace

scopes/ada-command-center/...
    imports AdaStorageNamespace
```

This is a cross-product ownership leak.

NEXT must determine:

```text
generic namespace fields
product-specific namespace values
application versus sub-scope semantics
local root derivation
Blob prefix derivation
interaction with existing storage topology
```

Do not rename `tool_namespace` or create a new abstraction before consumer inventory proves the
required shape.

## UNVERIFIED / AFTER NEXT

```text
dual-app real durable Source smoke
restart/readback against selected Storage target
Command Center explicit resource preparation
current-head artifact qualification
```

## Separate

Retention, cleanup/GC, Azure production, Entra and KPI/Collector remain outside this increment.
