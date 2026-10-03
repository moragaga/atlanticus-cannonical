# ADA Web — Session, Tool User Snapshot and Operational Consumption

Estado: **REFINED DESIGN / PLANNED INTEGRATION**

## Superseded session design

SUPERSEDED as target:

```text
login
→ read promoted user
→ query operational assignment
→ join operational catalog
→ join profiles
→ assemble session
```

The underlying durable contracts can still exist, but runtime login should not perform these joins.

## Current target

```text
identity provider
→ resolve user identity
→ read one Tool-specific users-runtime snapshot
→ establish session
```

`users-runtime` is the read authority for the current Tool.

## Runtime snapshot

Conceptual required data:

```text
identity
enabled
resolved profile
resolved operational
```

Operational structure is always present:

```text
area      {id, label}
position  {id, label}
group     {id, label}
```

Unconfigured values are `null`.

## Access

The user snapshot supplies `profile.id`.

ADA Access remains:

```text
profile_key -> access_keys
```

and can be held in memory/cache as a small shared projection.

Do not duplicate the complete access list into every user unless a later measured need justifies it.

## Session refresh

A page reload re-resolves the Tool user snapshot.

Server-side immediate session revocation remains a separate production-hardening concern.

## Warmup

The prior requirement to warm Profiles + Operational catalogs specifically for login is SUPERSEDED by the denormalized users-runtime target.

A cache/warmup may still be useful for other consumers, but it is not required to establish the session contract.

## Recovery

Immediate:

```text
Tool Users Recovery Snapshot
→ users-runtime
```

Future:

```text
Global Users + Tool Membership + Profiles + Operational
→ join
→ users-runtime
```

The future join is PLANNED and intentionally deferred.
