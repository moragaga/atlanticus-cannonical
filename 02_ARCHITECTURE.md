# Atlanticus — Architecture

Estado: **CURRENT — TOOL OWNERSHIP + USERS CUTOVER IMPLEMENTED; NAVIGATION REFINEMENT PLANNED**

## Regla principal

Atlanticus es modular y reusable. ADA y Command Center son consumidores.

## Generic Web capabilities

```text
Source Core / Local / Blob
Projection Core
Storage Namespace
Storage Topology
Users
Profiles
Navigation
Manager
Master Projection
```

## Storage Namespace CURRENT

```text
StorageNamespace(application_namespace, scope_namespace)

application_prefix = <application_namespace>
scope_prefix       = <application_namespace>/<scope_namespace>
```

## ADA ownership CURRENT

### Application-global
```text
Users identity registry
```

### Tool-scoped Blob
```text
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

## Cosmos CURRENT

```text
one Cosmos database/runtime boundary per Tool
```

Por esto Users runtime no repite application/tool routing.

## Users CURRENT

```text
Global UserIdentity
+ ToolUserMembership
+ Profiles
+ Operational
→ materialization
→ RuntimeUser
```

Global identity no contiene `profile_key` ni `enabled`.

RuntimeUser contiene identity, enabled, profile resuelto y operational resuelto.

## Runtime/session CURRENT

```text
identity provider
→ users-runtime.resolve(identity)
→ RuntimeUser
→ session / Manager principal / Navigation principal
```

## Access

ADA Access sigue separado:

```text
profile_key -> access_keys
```

## Recovery CURRENT

```text
Tool recovery snapshot
→ complete users-runtime REPLACE
```

No RESTORE parcial.

## Local identity

Jane/John conservan paleta local por subject. No es Global UserIdentity.

## Navigation — CURRENT vs TARGET

CURRENT:

```text
allowed_profiles empty
→ unrestricted/public
```

TARGET:

```text
access_mode = PUBLIC | RESTRICTED

PUBLIC
    allowed_profiles = []

RESTRICTED + []
    root/local only

RESTRICTED + [profiles]
    selected profiles + root/local
```

Misma semántica para menú, autorización de URL y UI Manager.

## Clean cutover rule

```text
contracts before consumers
clean replacement
no legacy aliases
no dual write
no compatibility storage path
```
