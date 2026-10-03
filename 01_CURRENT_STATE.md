# Atlanticus — Current State

Estado: **CURRENT — TOOL-SCOPED USERS CUTOVER CLOSED; NAVIGATION ACCESS NEXT**

## Autoridad

```text
Implementation
moragaga/atlanticus@2f9b65c3ba2646d519abfb0bb49e095d6819d185

Decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical before replacement
moragaga/atlanticus-cannonical@c530eec42e792ed9dc8aef4efbc07a0b94d6f1c9
```

## CLOSED / VERIFIED

```text
ADA-TOOL-SCOPED-SOURCE-OWNERSHIP
ADA-USERS-GLOBAL-IDENTITY
ADA-TOOL-USER-MEMBERSHIP
ADA-USERS-RUNTIME
ADA-USERS-RECOVERY-SNAPSHOT
MASTER-PROJECTION-USERS-REPLACE
ADA-MANAGER-RUNTIMEUSER-CONSUMPTION
LOCAL-JANE-JOHN-AVATAR-PALETTES
```

## Ownership CURRENT

```text
application namespace
└── users/users.json.gz
    └── UserIdentity[]

tool namespace
├── Source Navigation
├── Source Profiles
├── Source ADA Access
├── Source Operational
├── Source Tools
├── Source KPI Registry
├── Source KPI Definitions
├── users/memberships.json.gz
└── users/recovery/
    ├── snapshots/
    ├── audit/
    └── replace-before/

Cosmos propio de la Tool
└── users-runtime
    └── RuntimeUser
```

## Users CURRENT

```text
UserIdentity
    user_id
    issuer
    subject_id
    display_name
    email

ToolUserMembership
    user_id
    profile_key
    enabled

RuntimeUser
    identity
    enabled
    profile {id,label,background_color,text_color}
    operational {area,position,group}
```

`root` es asignable a un usuario administrado.

`guest` y `local` no son perfiles administrados asignables.

Jane/John mantienen sus paletas locales propias sin persistir colores en Global Users.

## users-runtime CURRENT

```text
id            = user_id
partition_key = user_id
```

No contiene ni consulta por `application_key`/`tool_key`.

## Recovery CURRENT

```text
Global Users
+ Tool Membership
+ Profiles
+ Operational
→ RuntimeUser[]
→ Tool-scoped recovery snapshot
→ complete REPLACE users-runtime
```

Recovery no modifica Global Users ni Tool Membership. No existe RESTORE parcial legacy.

## Qualification focal reportada por el usuario

```text
Users Core                    44 passed
Users Blob                     7 passed
Users Cosmos                   7 passed
Master Projection             53 passed
ADA Configuration Manager     65 passed
ADA Generic Application      199 passed
```

## CURRENT / OPEN — Navigation

CURRENT:

```text
allowed_profiles=[]
→ public/unrestricted
```

DECIDED / PLANNED:

```text
PUBLIC
    acceso ordinario

RESTRICTED + []
    sólo root/local implícitos

RESTRICTED + [profiles]
    perfiles seleccionados + root/local
```

Debe implementarse también en la UI del Manager.

## PLANNED después de Navigation

```text
1. current-head artifact generation/qualification
2. .env.detail full audit
3. distribution regeneration
4. isolated consumer qualification
5. ADA Generic real configuration with Docker Storage/Cosmos
6. resume Operaciones Integradas end-to-end
7. backend processes as separate increments
8. Collector / Time Status / UI
```

## OPEN / SEPARATE

```text
macOS host sync / rcssmin wheel           BLOCKED
/health/ready functional checks           OPEN
Python 3.14.7 / Trixie                    PLANNED
production Azure / Entra                  UNVERIFIED
Command Center distributed runtime        separate
Alarm integration                         separate
KPI backend/history/timeseries work       separate
```
