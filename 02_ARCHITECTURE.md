# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA.

## Ownership y scopes

`scopes/` contiene composiciones y capacidades específicas de producto/proyecto cuando
corresponde.

Una capability bajo `scopes/ada` puede consumir infraestructura genérica Atlanticus sin
transferir su ownership al core genérico.

```text
Atlanticus generic infrastructure/capabilities
    Source / Projection / Manager / Navigation / Users / Profiles / ...

ADA-specific capabilities
    Tools / KPI Configuration / KPI Definition / ADA Access / ...
```

No generalizar una capability sólo porque reutiliza contracts genéricos.

## Planos principales

### Platform

Capacidades transversales:

- backend;
- connectivity;
- integrations;
- web.

`web/` es frontera para Flask/Dash, JavaScript/CSS, composición Web, server-side Python con
responsabilidad Web y capabilities Web reutilizables.

Connectivity es dual-use y no adquiere ownership funcional.

### Configuration / Administration

Manager administra configuración, authoring, validation, publication, history y projection
actions para domains que realmente sean Configuration Sources.

Source genérico:

```text
web/capabilities/source/
```

Projection genérica exact-release:

```text
web/capabilities/projection/core
```

Manager consume esos contracts directamente.

No toda entidad administrable debe convertirse en Manager/Source/Projection.

### Entity lifecycle

Users pertenece a lifecycle global de entidad, no a Configuration Source.

```text
Global Users
        │
        ├── durable registry: UsersRegistryStore / Blob
        ├── promoted/runtime: UsersAdministrationStore + UsersRuntimeStore / Cosmos
        ├── optional directory discovery: UsersDirectoryReader
        └── profile reference: profile_key -> Profiles core
```

Users no vuelve a ser Source/Projection sólo porque su administración use UI.

### Operational Data

Operational Data conserva ownership separado para sources, producers, processes, planner y
materialization.

### ADA Runtime

ADA Generic compone experiencia operacional y consume capabilities Atlanticus y contracts
ADA-specific ya resueltos.

ADA-specific authorization puede consumir contracts genéricos sin convertirse en dependency
del core Atlanticus.

## Manager vs ADA Generic

```text
Manager      = administrar Configuration Source/Projection
ADA Generic  = consumir configuración y materializar experiencia operacional
Users Admin  = administrar lifecycle de Users globales
```

No fusionar responsabilidades por conveniencia de UI.

## Configuration vs Data

```text
CONFIGURATION DETERMINES EXISTENCE
DATA DETERMINES STATE
ENTITY REGISTRY DETERMINES GLOBAL USER LIFECYCLE
```

## Source vs Projection

Source y Projection son responsabilidades separadas.

```text
Source     = Local | Blob | provider equivalente
Projection = Local | Cosmos | provider equivalente
```

Projection representa un `SourceReleaseRef` concreto mediante `ProjectionTarget`.

```text
ProjectionTarget
= SourceKey
+ SourceReleaseRef
+ dependencies
```

Source current nunca se determina desde Cosmos.

`ProjectionTarget.dependencies` representa dependencias semánticas exactas entre
projections cuando existen realmente.

No existe un orden global obligatorio de todas las projections.

Estas reglas no convierten Users en Configuration Source.

## Contrato único de Manager

Cada `ManagerModule` declara el contract generic CURRENT.

No existe una segunda familia `exact_*`.

Manager consume:

```text
get_status(source_key)
select_current_target(source_key)
project(ProjectionTarget)
```

No existe adapter Manager hacia identidad textual de revision.

## Workspace genérico

`ManagerWorkspace` conserva owner, payload local, SourceSnapshot base, revision local y
metadata de guardado.

No reconstruir `ProjectionTarget` desde revision.

## Navigation CURRENT

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

Navigation continúa generic.

Durable:

```text
allowed_profiles = tuple de profile keys
```

Navigation Configuration consume:

```text
ProfileCatalog
ProfileDefinition
NavigationProfileCatalogProvider
```

No depende de:

```text
Users
ADA Access
Profiles Configuration
```

## Users CURRENT

Estructura:

```text
users/
├── activity
├── blob
├── core
└── cosmos
```

No existen CURRENT:

```text
users/configuration
users/projection-cosmos
compositions/users-manager
```

Strong identity:

```text
(issuer, subject_id)
user_id = build_user_key(issuer, subject_id)
```

Contrato CURRENT:

```text
UserRecord.profile_key
EffectiveUser.profile_key
```

Users depende de Profiles core para validar perfiles.

Global User no contiene:

```text
ADA access_keys
Navigation configuration
Tools/KPI configuration
otro estado application-specific
```

SUPERSEDED / REMOVED:

```text
authority_key
authority.py
basic|root authority mini-contract
```

Managed users pueden referenciar perfiles existentes en `ProfileCatalog` salvo `local`.

`local` es runtime-only.

Durable registry:

```text
BlobUsersRegistryStore
atlanticus_users_registry / schema 2
```

Promoted/runtime:

```text
CosmosUsersStore
atlanticus_user / schema 2
```

Session snapshot:

```text
v4
```

Identity autenticada sin promoted record continúa READY; promoted disabled continúa
USER_DISABLED / 403.

## Profiles CURRENT

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-WEB-SURFACE
CLOSED / VERIFIED / CURRENT

PROFILES-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT
```

Estructura:

```text
profiles/core
profiles/configuration
profiles/projection-local
profiles/projection-cosmos
web/compositions/profiles-manager
```

Ownership:

```text
profiles/core
    ProfileDefinition
    ProfileCatalog

profiles/configuration
    ProfilesConfiguration
    Source lifecycle
    Projection builder / serializer contract
```

Projection:

```text
ProjectionRecord[ProfileCatalog]
```

System profiles se reconstruyen desde código. La persistencia conserva configured profiles.

## ADA Access CURRENT

```text
ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT
```

ADA Access es application-specific bajo `scopes/ada`.

Ownership CURRENT:

```text
profile_key -> access_keys
```

No posee:

```text
user_id -> profile_keys
UserProfileAssignment
```

Contracts:

```text
ProfileAccessGrant
EffectiveAdaAccess(profile_key, access_keys)
AdaAccessConfiguration
AdaAccessSourceService
AdaAccessProjectionBuilder
```

Source schema:

```text
2
```

Projection payload:

```text
AdaAccessConfiguration
```

Dependencia exacta:

```text
Profiles ProjectionTarget
        ↓
ADA Access ProjectionTarget
```

La persistencia física de ADA Access Projection sigue OPEN.

## Tools CURRENT

Tool Configuration conserva semántica ADA y consume infraestructura genérica
Source/Projection.

## KPI Configuration CURRENT

Dependencia exacta:

```text
Tool ProjectionTarget
        ↓
KPI Configuration ProjectionTarget
```

## KPI Definition CURRENT

Dependencia exacta:

```text
KPI Configuration ProjectionTarget
        ↓
KPI Definition ProjectionTarget
```

## ADA Configuration Manager CURRENT

La aplicación final existente no debe confundirse con la disponibilidad de compositions
reusables individuales.

Profiles dispone de `profiles-manager`.

Users sigue fuera del modelo Source/Projection Manager.

La composición final de todas las superficies administrativas sigue separada.

## Reglas congeladas

```text
LEGACY                      REMOVE
ADAPTERS / SHIMS / ALIASES FORBIDDEN
DOUBLE CONTRACT             FORBIDDEN
OLD SCHEMA READERS          FORBIDDEN IN CURRENT RUNTIME
revision -> ProjectionTarget reconstruction REMOVE
expected_source_revision    REMOVE

USERS
user -> profile_key

USERS -> ADA-SPECIFIC STATE
FORBIDDEN

PROFILES
GENERIC ATLANTICUS FIRST-CLASS CAPABILITY

ADA ACCESS
profile_key -> access_keys

ADA ACCESS user_id -> profile_keys
REMOVED

NAVIGATION DURABLE AUTHORIZATION
PROFILE KEYS

NAVIGATION -> USERS
FORBIDDEN

NAVIGATION -> ADA ACCESS
FORBIDDEN

NAVIGATION CONFIGURATION -> PROFILES CORE
CURRENT
```

No reabrir Manager core, Source/Projection core ni ownership cerrado para acomodar el
siguiente provider de persistencia.
