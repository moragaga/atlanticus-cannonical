# Configuration Source Storage — Index

Estado: **CURRENT — CORE/NAMESPACE FROZEN; ADA PRODUCT MAPPING REFINEMENT NEXT**

## Closed contracts

```text
SOURCE CORE                         CLOSED / VERIFIED
SOURCE LOCAL                        CLOSED / VERIFIED
SOURCE BLOB                         CLOSED / VERIFIED
PROJECTION EXACT-RELEASE CORE       CLOSED / VERIFIED
STORAGE NAMESPACE                   CLOSED / VERIFIED
CROSS-PRODUCT NAMESPACE OWNERSHIP   CLOSED / VERIFIED
```

## Generic namespace CURRENT

```text
StorageNamespace(
    application_namespace,
    scope_namespace,
)

application_prefix
scope_prefix
application_blob_name(...)
scope_blob_name(...)
```

The generic capability is not changing.

## ADA product mapping — CURRENT implementation

```text
application_prefix
    Navigation
    Profiles
    ADA Access
    Operational
    Users Registry

scope_prefix
    Tools
    KPI Registry
    KPI Definitions
```

## ADA product mapping — DECIDED target

```text
application_prefix
    Users identity registry

scope_prefix
    Tools
    Profiles
    Navigation
    ADA Access
    Operational
    Tool User Membership
    KPI Registry
    KPI Definitions
    Tool Users Recovery Snapshot
```

This is a product-composition cutover, not a SourceStore redesign.

## Cosmos assumption

Each current ADA Tool uses its own Cosmos runtime/database boundary.

Do not add multi-tool collision defenses to Source Storage in this increment.

## NEXT

```text
ADA-TOOL-SCOPED-CONFIGURATION-AND-USER-RUNTIME
```

No Source Core changes, no legacy path readers, no dual-write.
