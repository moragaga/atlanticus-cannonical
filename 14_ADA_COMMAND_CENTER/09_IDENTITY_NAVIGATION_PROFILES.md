# ADA Command Center — Identity, Users, Profiles, Navigation and Manager

Estado: **CURRENT / CLOSED — CAPABILITY PARITY IMPLEMENTED**

Checkpoint:

```text
atlanticus@346e7ac7ba7c21eede8b524613a6adee7e839e55
```

## Identity

El host local usa identidad local.

Production Entra:

```text
PLANNED / UNVERIFIED
```

No inferir permisos productivos desde overrides locales.

## Generic capability model CURRENT

```text
Atlanticus Users       global identity + Tool membership + runtime/recovery
Atlanticus Profiles    profile definitions/configuration/projection
Atlanticus Navigation  route structure + PUBLIC/RESTRICTED authorization
Atlanticus Manager     administrative shell/authorization
Command Center         composition
```

## Users CURRENT

```text
UsersRegistryStore
ToolMembershipStore
UsersRuntimeStore
UsersDirectoryReader
```

Administration:

```text
UsersAdministrationService(
    registry=...,
    memberships=...,
    profiles=...,
    directory=...,
)
```

Physical ownership:

```text
Global Users Registry
    <application>/users/users.json.gz

Tool Membership
    <application>/<tool>/users/memberships.json.gz

RuntimeUser
    Tool-owned Cosmos users-runtime
```

## Runtime/recovery CURRENT

```text
Global Users
+ Tool Membership
+ Profiles
+ Operational
→ RuntimeUser[]
```

Recovery reemplaza únicamente runtime.

Users no es un `ProjectionDomain` ordinario.

## Profiles CURRENT

Command Center usa `compose_profiles_manager` y `ProfileCatalog` genéricos.

Local:

```text
shared local Source
in-process Projection
```

Durable:

```text
Blob Source
Cosmos Profiles Projection
```

## Navigation CURRENT

Persisted contract:

```text
access_mode = PUBLIC | RESTRICTED
allowed_profiles = (...)
```

Command Center importa:

```text
atlanticus.web.navigation.configuration.NAVIGATION_SOURCE_KEY
```

No existe autoridad local duplicada.

## Manager principal CURRENT

Cadena:

```text
IdentityProvider
→ AccessRuntime
→ UsersAccessResolver
→ UsersRuntime
→ ManagerPrincipalBinding
→ Manager
```

Reglas:

```text
managed root → administrative override
local override → sólo environment local confiable
disabled RuntimeUser → no principal válido
profile metadata/avatar → runtime/profile binding
```

## Manager

Manager authorization permanece separada de Navigation visibility.

Rutas genéricas:

```text
/manager/profiles
/manager/navigation
/manager/users
```

## Master Projection

Dominios ordinarios:

```text
Profiles
Navigation
Alarm Configuration
```

Users conserva operación especial snapshot/recovery. En durable composition, recovery/catalog permiten configurar `users.replace`.

## SUPERSEDED

```text
UsersAdministrationStore
UserRecord
users_promoted
promoted=
CosmosUsersStore as administration
local SourceKey('navigation')
old hard-coded navigation principal binding
```

No crear aliases de compatibilidad.

## OPEN / SEPARATE

```text
production Entra provider
production authorization mapping
real durable multi-service smoke
full Command Center qualifier blocked by upstream Tools contract duplication
```
