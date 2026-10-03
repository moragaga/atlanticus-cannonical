# ADA Generic — Current Composition

Estado: **CURRENT — TOOL-SCOPED SOURCE + USERS RUNTIME CUTOVER IMPLEMENTED**

## Composition root CURRENT

ADA Generic owns product composition for:

```text
settings
local/durable Manager composition
identity binding
Tool Projection resolution
operational render binding
ADA Master Projection composition
KPI Collector attachment
Web runtime lifecycle
```

## Namespace

```text
application_namespace = ADA_APPLICATION_NAMESPACE
scope_namespace       = ADA_TOOL_NAMESPACE
```

## Source ownership CURRENT

One Tool-scoped Source store is injected for:

```text
Navigation
Profiles
ADA Access
Operational
Tools
KPI Registry
KPI Definitions
```

Global Users Registry remains application-scoped.

Tool User Membership:

```text
<application>/<tool>/users/memberships.json.gz
```

Recovery:

```text
<application>/<tool>/users/recovery/...
```

## Cosmos CURRENT

One Cosmos runtime/database belongs to one Tool.

`users-runtime` uses `user_id` directly as item id/partition key and no app/tool routing fields.

## Session CURRENT

```text
Identity
→ users-runtime resolve
→ RuntimeUser
→ UsersRuntime
→ Manager/Navigation principal
```

## Master Projection CURRENT

Six Source Projection domains remain. Users is a special recovery operation.

## Current gap

Navigation still lacks explicit PUBLIC/RESTRICTED state.

## Distribution

Previously verified distributed artifacts predate this cutover.

Current HEAD must be regenerated/requalified after Navigation semantics close.
