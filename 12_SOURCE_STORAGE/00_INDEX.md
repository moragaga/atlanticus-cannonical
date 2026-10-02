# Configuration Source Storage — Index

Estado: **CURRENT — CORE FROZEN / CROSS-PRODUCT NAMESPACE-COMPOSITION CONVERGENCE NEXT**

## Closed contracts

```text
SOURCE CORE                         CLOSED / VERIFIED
SOURCE LOCAL                        CLOSED / VERIFIED
SOURCE BLOB                         CLOSED / VERIFIED
PROJECTION EXACT-RELEASE CORE       CLOSED / VERIFIED
MANAGER GENERIC HANDOFF             CLOSED / VERIFIED
TOOL PROJECTION PERSISTENCE         CLOSED / VERIFIED
```

## Packages CURRENT

```text
atlanticus-web-source
atlanticus-web-source-local
atlanticus-web-source-blob
```

These remain generic Atlanticus capabilities.

## Current namespace state

Existing helper:

```text
scopes/ada/web/storage/namespace
ada-web-storage-namespace
AdaStorageNamespace
```

is consumed by both ADA and Command Center.

Command Center imports it directly from the ADA scope.

That ownership is the next gap; Source Core is not the gap.

## NEXT

```text
SOURCE-NAMESPACE-AND-COMPOSITION-CONVERGENCE
```

Required outcome:

```text
one reusable namespace/composition contract where reuse is real
ADA product-specific naming remains in ADA
Command Center product-specific naming remains in Command Center
no Command Center dependency on ADA-owned generic-looking infrastructure
no SourceStore rewrite
```
