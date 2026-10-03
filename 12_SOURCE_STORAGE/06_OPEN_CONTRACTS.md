# Source Storage — Open Contracts

Estado: **CURRENT — CORE CLOSED / ADA PRODUCT MAPPING CUTOVER OPEN**

## Core Source — CLOSED / FROZEN

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
History
exact reads
integrity
```

## Projection handoff — CLOSED / FROZEN

```text
ProjectionTarget
exact source release provenance
dependency provenance
ProjectionStore.get_active/replace_active
```

## Storage namespace — CLOSED / FROZEN

```text
StorageNamespace(application_namespace, scope_namespace)
```

No generic API redesign is required.

## OPEN / NEXT — ADA product ownership mapping

Move the following Source consumers from application root to Tool scope:

```text
Profiles
Navigation
ADA Access
Operational
```

Add Tool-scoped membership/recovery ownership for Users.

Keep only global Users identity under application scope.

## User global contract — DECIDED target

Remove:

```text
profile_key
enabled
```

from global Users identity.

Tool-specific state moves to Tool User Membership.

## Recovery — immediate versus future

Immediate:

```text
Tool Users Recovery Snapshot
→ users-runtime
```

Future only:

```text
Global Users
+ Tool Membership
+ Profiles
+ Operational
→ join
→ users-runtime
```

The future join is not an acceptance gate for the next increment.

## Separate OPEN items

```text
retention / cleanup / GC
production Azure
Python 3.14.7 / Trixie
```
