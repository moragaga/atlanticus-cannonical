# ADA Command Center — Identity, Users, Profiles, Navigation and Manager

Estado: **BLOCKED / NEXT — Command Center debe alcanzar parity con las capabilities CURRENT y el patrón implementado en ADA**.

Checkpoint:

```text
atlanticus@6725237a19c4442fdfa1b32c3410c124e9348dbc
```

## Identity

El host local usa identidad local.

Production Entra permanece:

```text
PLANNED / UNVERIFIED
```

No inferir permisos productivos desde `administrative_override` local.

## Generic capability model CURRENT

```text
Atlanticus Users       global identity + Tool membership + runtime/recovery contracts
Atlanticus Profiles    profile definitions/configuration/projection
Atlanticus Navigation  route structure + PUBLIC/RESTRICTED authorization
Atlanticus Manager     administrative shell/authorization
product                composition
```

## Users CURRENT generic contract

```text
UsersRegistryStore
ToolMembershipStore
UsersRuntimeStore
UsersAdministrationService(
    registry=...,
    memberships=...,
    profiles=...,
    directory=...,
)
```

Physical ownership usado por ADA:

```text
Global Users Registry
    <application>/users/users.json.gz

Tool Membership
    <application>/<tool>/users/memberships.json.gz

RuntimeUser
    Tool-owned Cosmos users-runtime
```

## Command Center Users — BLOCKED

El Configuration Manager todavía usa API superseded:

```text
UsersAdministrationStore
UserRecord
users_promoted
promoted=
CosmosUsersStore como administración
```

Esto ya no compone contra Atlanticus Users CURRENT.

El próximo incremento debe reemplazarlo limpiamente; no crear aliases de compatibilidad.

## Profiles

Command Center ya usa `compose_profiles_manager` y `ProfileCatalog` genéricos.

Local:

```text
shared local Source
in-process Projection
```

Durable CURRENT:

```text
Blob Source
Cosmos Profiles Projection
```

No copiar `users-support` de ADA automáticamente. Compartir recurso físico sólo cuando exista compatibilidad y beneficio real.

## Navigation CURRENT generic contract

Navigation Configuration ya modela explícitamente:

```text
access_mode = PUBLIC | RESTRICTED
allowed_profiles = (...)
```

Command Center debe importar la autoridad `NAVIGATION_SOURCE_KEY` desde la capability genérica y dejar de declarar su propia constante equivalente.

Su `NavigationPrincipal` runtime también requiere revisión de paridad con ADA: presentation/profile metadata y semántica root/local no deben quedar hard-coded en una binding antigua.

## Manager

Manager authorization permanece separada de Navigation visibility y de cualquier Access específico de ADA.

## Master Projection

Command Center conserva como Source Projection domains:

```text
Profiles
Navigation
Alarm Configuration
```

Users no debe agregarse como domain normal sólo para copiar ADA; recovery/runtime tiene contrato propio.

## Regla de paridad

Copiar el **patrón de composición** que ADA ya usa para capabilities genéricas.

No copiar lógica específica de producto ADA dentro de Command Center.

## OPEN

```text
Command Center capability parity
production Entra provider
production authorization mapping
real durable smoke after parity
decision de si Command Center necesita Users Runtime/Recovery completo igual que ADA
```
