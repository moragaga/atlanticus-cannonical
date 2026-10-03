# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global — FROZEN

```text
uv; no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
one focus per increment
Git read-only unless explicit authorization
```

## ADA namespace ownership — CURRENT / CLOSED

```text
application-global
    Users identity registry

tool-scoped
    Tool Configuration
    Profiles
    Navigation
    ADA Access
    Operational
    Tool User Membership
    KPI Registry
    KPI Definitions
    Tool Users Recovery artifacts
```

## Users model — CURRENT / CLOSED

SUPERSEDED:
```text
global UserRecord owns profile_key + enabled
```

CURRENT:
```text
UserIdentity                   application-global
ToolUserMembership             Tool-scoped
RuntimeUser                    Tool runtime snapshot
```

`root` es asignable a usuarios administrados. `guest/local` no.

## users-runtime — CURRENT / CLOSED

```text
one Cosmos per Tool
item id       = user_id
partition key = user_id
```

No `application_key`/`tool_key` internos.

## Users recovery — CURRENT / CLOSED

```text
Tool snapshot
→ complete users-runtime REPLACE
```

Scope físico por Blob path. No RESTORE parcial.

## Master Projection Users — CURRENT / CLOSED

Users es operación especial de recovery, no Source Projection normal.

## Navigation authorization — DECIDED / PLANNED

CURRENT:
```text
allowed_profiles=[] → unrestricted
```

TARGET frozen:
```text
PUBLIC
RESTRICTED + []          root/local only
RESTRICTED + [profiles]  profiles + root/local
```

UI Manager debe editar modo explícito.

## Distribution order — REFINED

```text
1. Navigation explicit access contract + UI
2. current-head artifacts
3. .env.detail audit
4. distribution regeneration
5. isolated consumer
6. ADA real configuration/end-to-end
```

## Separate

```text
Python 3.14.7/Trixie
production Azure/Entra
Command Center runtime
Alarm
KPI backend/process work
```
