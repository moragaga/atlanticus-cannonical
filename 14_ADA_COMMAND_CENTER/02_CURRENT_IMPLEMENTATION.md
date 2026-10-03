# ADA Command Center — Current Implementation

Estado: **CURRENT — GENERIC CAPABILITY PARITY IMPLEMENTED IN MAIN**

Checkpoint:

```text
moragaga/atlanticus@346e7ac7ba7c21eede8b524613a6adee7e839e55
```

## Generic Application

Rol:

```text
real Command Center Web product composition root
```

Composition incluye:

```text
Home
Identity
Users Runtime resolution
Projected Navigation
Navigation authorization
Manager principal binding
Manager surface/modules
Master Projection independent surface
Command Center pages
Manager pages
```

## Configuration Manager

El host soporta:

```text
ADA_MANAGER_PERSISTENCE_PROVIDER=local
ADA_MANAGER_PERSISTENCE_PROVIDER=durable
```

## Administration parity CURRENT

Command Center compone directamente:

```text
compose_profiles_manager
compose_navigation_manager
compose_users_manager
compose_users_projection_manager
```

Users usa:

```text
UsersAdministrationService(
    registry=...,
    memberships=...,
    profiles=...,
    directory=...,
)
```

No queda autorizado mantener la API superseded como compatibilidad.

## Durable stores CURRENT

```text
Blob Source
Cosmos Alarm Configuration Projection
Cosmos Profiles Projection
Cosmos Navigation Projection

Blob Users Registry
Blob Tool Membership
Cosmos Users Runtime
Blob Users Recovery snapshots/audit

Tool Catalog on Storage
```

## Users physical ownership CURRENT

```text
Registry
    <application>/users/users.json.gz

Membership
    <application>/<tool>/users/memberships.json.gz

Recovery
    <application>/<tool>/users/recovery/...

Runtime
    Cosmos users-runtime
```

## Storage namespace CURRENT

Command Center usa:

```text
atlanticus.web.storage.namespace.StorageNamespace
```

Product namespace:

```text
StorageNamespace(
    "conciencia_situacional",
    "command-center",
)
```

## Navigation CURRENT

Usa la autoridad:

```text
atlanticus.web.navigation.configuration.NAVIGATION_SOURCE_KEY
```

Persisted access:

```text
PUBLIC
RESTRICTED + allowed_profiles
```

Runtime principal binding resuelve metadata de profile, root override y local confiable siguiendo el contrato genérico ya usado por ADA.

## Master Projection CURRENT

Dominios ordinarios:

```text
Profiles
Navigation
Alarm Configuration
```

Users:

```text
special snapshot/recovery operation
not ProjectionDomain
```

Cuando durable runtime inyecta recovery y snapshot catalog, el backend de Master puede ejecutar `users.replace`.

## Qualification focal de este hito

```text
configuration-manager   31 passed
generic-application     12 passed
catalog-manager          8 passed
Ruff                    PASS
```

Los cuatro lockfiles del qualifier Web fueron normalizados e integrados en este checkpoint.

## BLOCKED separado

El qualifier completo no está GREEN:

```text
catalog            7 failed / 5 passed
discovery-cosmos   1 failed / 33 passed
```

La causa observada es la coexistencia de tipos `ada.web.tools.*` y `ada.contracts.tools.*`.

No corresponde corregir ADA dentro de este hito.

## Alarm backend

Permanece físicamente bajo:

```text
scopes/ada-command-center/backend
```

El próximo frente debe evaluar su extracción completa como `ada-alarm-engine`, removiendo/invirtiendo dependencias hacia Web.
