# Users — Global Identity, Tool Runtime Snapshot and Recovery

Estado: **CURRENT OLD IMPLEMENTATION / REFINED TARGET DECIDED / GRANULAR REBUILD PLANNED**

## CURRENT implementation

Current code still has:

```text
Global UserRecord
    identity
    profile_key
    enabled
```

Blob Registry is application-global.

Cosmos `users-runtime` stores promoted users.

Existing approved snapshot/recovery machinery remains implemented evidence until the clean cutover replaces the user contract.


## Existing recovery mechanics preserved as implemented evidence

Until the cutover replaces the user payload contract, the existing recovery capability remains real implementation:

```text
approved immutable snapshots
explicit preview/validation
RESTORE distinct from REPLACE
before-images
audit started/completed/failed
digest/content checks
```

The next redesign may adapt these mechanics to the Tool Users Recovery Snapshot, but must not silently claim they already operate on the new Tool-scoped snapshot schema.

## REFINED target — global identity

Global durable Users becomes identity-only:

```text
user_id
issuer
subject_id
display_name
email
```

It is shared across Tools in the application namespace.

## REFINED target — Tool membership

Each Tool owns durable membership:

```text
user_id
profile_key
enabled
```

under its Tool namespace.

## REFINED target — Tool users-runtime

`users-runtime` is a Tool-specific denormalized snapshot optimized for login/session reads.

Minimum conceptual shape:

```text
identity
enabled
profile
    id
    label
    colors
operational
    area.id/label
    position.id/label
    group.id/label
```

Operational fields are present even when unconfigured; values are `null`.

## Immediate recovery strategy — DECIDED

Use a complete Tool Users Recovery Snapshot to restore the Tool's `users-runtime`.

The snapshot belongs to the Tool scope because profile/enabled/operational values are Tool-specific.

This avoids introducing complex joins into the next increment.

## Future reconstruction — PLANNED ONLY

```text
Global Users
+ Tool Membership
+ Profiles
+ Operational
→ join by stable IDs
→ users-runtime
```

This is not a requirement for the next cutover and must not block returning to ADA UI configuration.

## Master Projection

Master Projection must treat the Tool Users Recovery Snapshot as the immediate recovery authority for users-runtime after the cutover.

Do not infer that the current Master Users workflow already implements this new target.

Status:

```text
DECIDED / PLANNED / NOT YET IMPLEMENTED
```

## Invariants

```text
runtime does not read Blob during login
global user has no Tool profile/enabled
Tool membership references global user_id
profile and operational labels may be denormalized into runtime snapshot
Access permissions remain separate profile -> access mapping
no legacy dual user model
```
