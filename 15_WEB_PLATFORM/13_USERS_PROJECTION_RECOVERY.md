# Users — Global Identity, Tool Runtime Snapshot and Recovery

Estado: **CURRENT / IMPLEMENTED / CLOSED**

## Global identity CURRENT

```text
UserIdentity
    user_id
    issuer
    subject_id
    display_name
    email
```

## Tool Membership CURRENT

```text
ToolUserMembership
    user_id
    profile_key
    enabled
```

## Materialization CURRENT

```text
Global Users
+ Tool Membership
+ Profiles
+ Operational
→ RuntimeUser[]
```

RuntimeUser contains identity, enabled, profile and operational.

## Recovery snapshot CURRENT

Physical scope:

```text
<application>/<tool>/users/recovery/snapshots
```

Snapshot body does not duplicate `application_key` or `tool_key`.

## Cosmos users-runtime CURRENT

```text
item id       = user_id
partition key = user_id
```

One Cosmos runtime boundary belongs to one Tool.

## Recovery operation

```text
validate
before-image
audit started
replace_all
verify exact match
audit completed/failed
```

Complete REPLACE only.

Global Users and Membership are not mutated.

## Master Projection CURRENT

Users is a special snapshot/recovery operation, not a seventh Source Projection domain.

Self-contained snapshot is not gated on Profiles CURRENT.

## Future

Continuous/event-driven materialization may be considered later if required; it is not a blocker now.
