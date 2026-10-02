# Web Platform — Current Gaps

Estado: **CURRENT — SOURCE OWNERSHIP CLOSED / DURABLE RUNTIME VALIDATION PRIMARY GAP**

## CLOSED / CURRENT

```text
generic Source Core/Local/Blob
generic Storage Namespace
generic Manager compositions used by current products
Command Center durable Manager composition
shared Master Projection engine
ADA Master Projection product composition
Command Center Master Projection product composition
cross-product namespace ownership convergence
```

## Storage namespace CURRENT

Generic owner:

```text
web/capabilities/storage/namespace
atlanticus-web-storage-namespace==0.1.0
StorageNamespace(application_namespace, scope_namespace)
```

Product-independent semantics:

```text
application scope
sub-scope
local roots
Blob prefixes
projection root derivation
```

Product-specific values stay in their product compositions.

No Command Center dependency on ADA-owned storage namespace infrastructure remains.

## Primary gap

```text
DUAL-APP-DURABLE-RUNTIME-SMOKE
```

Reason:

Unit/package qualification is complete, but the full durable application compositions have not yet been validated together against selected Storage/Cosmos targets.

The smoke must establish that:

```text
ADA Generic durable composition starts
Command Center durable composition starts
storage/cosmos bindings resolve
namespace physical identities are preserved in running composition
restart/readback is valid where required by the selected smoke contract
```

## Secondary gaps after runtime smoke

```text
current-head distribution regeneration
ADA lift using Atlanticus distribution
Command Center resource-preparation parity only if runtime evidence requires it
production identity / Azure
```

## Deferred architecture

```text
scope tooling topology normalization
backend distribution tooling normalization
Operational Data tooling relocation/normalization
Python 3.14.7 / Trixie
```

These are not blockers for the durable runtime smoke.
