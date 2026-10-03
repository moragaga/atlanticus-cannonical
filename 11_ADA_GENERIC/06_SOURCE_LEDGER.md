# ADA Generic — Source Ledger

Estado: **AUDIT LEDGER / USERS TOOL RUNTIME CUTOVER 2026-10-03**

## Authorities

```text
Implementation  moragaga/atlanticus@2f9b65c3ba2646d519abfb0bb49e095d6819d185
Decisions       moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical base  moragaga/atlanticus-cannonical@c530eec42e792ed9dc8aef4efbc07a0b94d6f1c9
```

Historical Navigation local, Tool persistence, Operational bootstrap, Collector and distributed runtime evidence remain historical.

## Current checkpoint

Implemented:

```text
Tool-scoped Navigation/Profiles/Access/Operational Sources
global identity-only Users registry
Tool User Membership
RuntimeUser Cosmos
Tool recovery snapshots
Master Projection Users REPLACE
Manager/session RuntimeUser consumption
local Jane/John palettes preserved
```

SUPERSEDED:

```text
application_source for Navigation/Profiles/Access/Operational
global UserRecord owns profile/enabled
promoted users Cosmos model
partial RESTORE
users_promoted
```

Qualification focal:

```text
ADA Generic Application 199 passed
```

Current conflict:

```text
Navigation allowed_profiles empty → unrestricted
```

Accepted NEXT requires explicit PUBLIC/RESTRICTED.

The historical distributed artifact does not qualify current HEAD.
