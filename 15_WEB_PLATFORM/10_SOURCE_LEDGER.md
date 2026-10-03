# Web Platform — Source Ledger

Estado: **AUDIT LEDGER / USERS TOOL RUNTIME CUTOVER CHECKPOINT 2026-10-03**

## Current authority

```text
Implementation  moragaga/atlanticus@2f9b65c3ba2646d519abfb0bb49e095d6819d185
Decisions       moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical base  moragaga/atlanticus-cannonical@c530eec42e792ed9dc8aef4efbc07a0b94d6f1c9
```

## Preserved capability boundaries

```text
Navigation Configuration -> Users dependency forbidden
Navigation -> ADA Access dependency forbidden
Manager authorization separate
Profiles generic
ADA Access product-owned
```

## Users cutover CURRENT

```text
Global UserIdentity
ToolUserMembership
RuntimeUser
UsersRuntime session
Tool recovery snapshot
CosmosUsersRuntimeStore
```

Tool Source ownership is current for all Tool-varying ADA Web configuration.

## Local presentation CURRENT

```text
Jane #C85D91
John #3778C2
```

Not Global UserIdentity data.

## Qualification focal

```text
Users Core             44 passed
Users Blob              7 passed
Users Cosmos            7 passed
Master Projection      53 passed
```

## Navigation conflict ledger

CURRENT:

```text
allowed_profiles empty → unrestricted
```

DECIDED target:

```text
PUBLIC
RESTRICTED + []
RESTRICTED + [profiles]
```

Status: **OPEN / NEXT**.
