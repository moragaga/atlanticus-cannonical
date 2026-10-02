# Configuration Source Storage — Index

Estado: **CURRENT — CORE + NAMESPACE FROZEN / DUAL-APP DURABLE RUNTIME SMOKE NEXT**

## Closed contracts

```text
SOURCE CORE                         CLOSED / VERIFIED
SOURCE LOCAL                        CLOSED / VERIFIED
SOURCE BLOB                         CLOSED / VERIFIED
PROJECTION EXACT-RELEASE CORE       CLOSED / VERIFIED
MANAGER GENERIC HANDOFF             CLOSED / VERIFIED
TOOL PROJECTION PERSISTENCE         CLOSED / VERIFIED
STORAGE NAMESPACE                   CLOSED / VERIFIED
CROSS-PRODUCT NAMESPACE OWNERSHIP   CLOSED / VERIFIED
```

## Packages CURRENT

```text
atlanticus-web-source
atlanticus-web-source-local
atlanticus-web-source-blob
atlanticus-web-storage-namespace
```

These are generic Atlanticus capabilities.

## Namespace CURRENT

Generic package:

```text
web/capabilities/storage/namespace
atlanticus-web-storage-namespace==0.1.0
atlanticus.web.storage.namespace.StorageNamespace
```

Frozen logical shape:

```text
StorageNamespace(
    application_namespace,
    scope_namespace,
)
```

Derived identities:

```text
application_prefix
scope_prefix = <application_namespace>/<scope_namespace>
local_application_root(base_root)
local_scope_root(base_root)
local_projection_root(base_root)
application_blob_name(relative_path)
scope_blob_name(relative_path)
```

The second segment is intentionally named `scope_namespace`.

Reason:

```text
ADA uses the second segment as Tool namespace
Command Center uses the second segment as product sub-scope
```

The generic contract therefore does not encode Tool semantics.

## Ownership CURRENT

The previous ADA-owned helper:

```text
scopes/ada/web/storage/namespace
ada-web-storage-namespace
AdaStorageNamespace
```

is superseded.

ADA and Command Center now consume:

```text
atlanticus.web.storage.namespace.StorageNamespace
```

Command Center no longer imports storage namespace infrastructure from the ADA scope.

## Preserved physical identities

The convergence did not redesign Source or Projection persistence.

Preserved:

```text
ADA application prefix
ADA <application>/<tool> scope prefix
Command Center conciencia_situacional/command-center prefix
local application/scope roots
local projection root = <scope-root>/projections
Blob application/scope paths
Cosmos Tool Projection namespace_key
SourceStore behavior
Projection exact-release behavior
```

## NEXT

```text
DUAL-APP-DURABLE-RUNTIME-SMOKE
```

Required outcome:

```text
ADA Generic starts with durable composition
Command Center starts with durable composition
selected Storage/Cosmos bindings resolve
namespace-derived paths/partition identities remain valid at runtime
restart/readback is exercised where the selected smoke contract requires it
```

Current-head distribution regeneration remains after this smoke.
