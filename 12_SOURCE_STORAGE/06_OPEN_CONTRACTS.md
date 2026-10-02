# Source Storage — Open Contracts

Estado: **CURRENT — CORE + NAMESPACE CLOSED / DURABLE RUNTIME SMOKE OPEN**

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

## Storage namespace — CLOSED / FROZEN

Current generic owner:

```text
web/capabilities/storage/namespace
atlanticus-web-storage-namespace==0.1.0
```

Public contract:

```text
StorageNamespace(
    application_namespace: str,
    scope_namespace: str,
)
```

Frozen derivations:

```text
application_prefix = application_namespace
scope_prefix = application_namespace + "/" + scope_namespace

local_application_root(base_root)
local_scope_root(base_root)
local_projection_root(base_root)

application_blob_name(relative_path)
scope_blob_name(relative_path)
```

Frozen validation:

```text
namespace segments are non-empty text
no surrounding whitespace
"." and ".." are invalid segments
"/", "\\" and NUL are invalid inside a segment
local base roots must be absolute
Blob relative paths must remain safe relative POSIX paths
```

## Product mappings — CLOSED / FROZEN

ADA:

```text
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE
    ↓
StorageNamespace(
    application_namespace=<ADA_APPLICATION_NAMESPACE>,
    scope_namespace=<ADA_TOOL_NAMESPACE>,
)
```

The external ADA environment contract remains unchanged.

Command Center:

```text
StorageNamespace(
    "conciencia_situacional",
    "command-center",
)
```

The resulting physical prefix remains:

```text
conciencia_situacional/command-center
```

## Superseded

```text
scopes/ada/web/storage/namespace
ada-web-storage-namespace
AdaStorageNamespace
tool_prefix
tool_blob_name(...)
local_tool_root(...)
```

There is no compatibility shim or legacy alias.

The generic replacements are:

```text
StorageNamespace
scope_prefix
scope_blob_name(...)
local_scope_root(...)
```

`local_projection_root(...)` remains part of the generic contract.

## OPEN / NEXT

```text
DUAL-APP-DURABLE-RUNTIME-SMOKE
```

The smoke must validate composition outside isolated unit tests:

```text
ADA Generic durable startup
Command Center durable startup
selected Storage/Cosmos bindings
namespace-derived Source roots
Tool Projection namespace identity
product-specific durable paths
restart/readback where required by the agreed smoke contract
```

## OPEN / AFTER NEXT

```text
current-head distribution regeneration
ADA consumption of Atlanticus distribution
Command Center explicit resource preparation if runtime evidence reveals it as a blocker
```

## Separate

```text
production Entra
Azure production qualification
retention / cleanup / GC
KPI Runtime / Historian / Delivery
Collector / browser stores / UI
Python 3.14.7 / Trixie migration
tooling topology normalization
```
