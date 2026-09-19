# Web Platform — Users / Profiles / Access / Navigation Capability Boundary

Estado: **CURRENT DECISION / REFINED AFTER ADA ACCESS PERSISTENCE AND PROFILES MANAGER ADOPTION**

## Propósito

Fijar la frontera CURRENT entre:

```text
Global Users
Generic Profiles
Application-specific ADA Access
Generic Navigation
Manager administrative shell
```

sin reintroducir Source/Projection falsos en Users, estado ADA-specific en Global User,
catálogos paralelos de Profiles ni contracts legacy.

## Autoridad de implementación

```text
moragaga/atlanticus@415c8263c15bae2b5d3c01b734b0f1e0101a7242
```

Parent:

```text
9b623d67e253413f6b0d894e10d9cc15735b553d
```

Tree:

```text
ff81bcd74b5e844604ca656562f9aaf398946d67
```

## Estado del frente

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-WEB-SURFACE
CLOSED / VERIFIED / CURRENT

PROFILES-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

PROFILES-ADA-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT
```

## Regla principal

### Profiles

```text
profile definition + catalog + configuration + Source + Projection
```

Generic Atlanticus.

### Users

```text
identity + lifecycle + user -> profile_key
```

Generic Atlanticus.

No contiene ADA-specific access state.

### ADA Access

```text
profile_key -> ADA access_keys
```

Application-specific.

### Navigation

```text
allowed_profiles = profile keys
```

Generic. No importa Users ni ADA Access para resolver rutas.

## Users CURRENT

Contrato durable/effective:

```text
UserRecord.profile_key
EffectiveUser.profile_key
```

Lifecycle administrativo CURRENT:

```text
UsersAdministrationService
```

Managed profiles:

```text
cualquier profile key existente en ProfileCatalog
excepto local
```

`local`:

```text
LOCAL-RUNTIME ONLY
```

No existe CURRENT:

```text
authority_key
User authority basic|root mini-contract
administrator -> root alias
Users Manager Source/Projection module
users-manager composition
```

Users consume `ProfileCatalog` para validación y opciones administrativas.

## Profiles CURRENT

```text
profiles/core
profiles/configuration
profiles/projection-local
profiles/projection-cosmos
web/compositions/profiles-manager
```

`ProfileDefinition`:

```text
key
label
background_color
text_color
```

System profiles:

```text
basic
root
guest
local
```

Configured profiles conservan stable keys.

La Web surface de Profiles es CURRENT.

Profiles Manager es CURRENT y ADA Configuration Manager ya consume su `ManagerModule`.

Services de Profiles se registran a través del `ManagerModule.web_module` existente sobre
el `ServiceRegistry` real de Atlanticus Web.

No existe registry temporal ni contract paralelo de integración.

## ADA Access CURRENT

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration
scopes/ada/web/access/projection-local
scopes/ada/web/access/projection-cosmos
```

Contracts:

```text
ProfileAccessGrant
EffectiveAdaAccess(profile_key, access_keys)
AdaAccessConfiguration(profile_access=...)
AdaAccessSourceService
AdaAccessProjectionBuilder
```

Ownership:

```text
profile_key -> access_keys
```

No existe CURRENT:

```text
UserProfileAssignment
user_id -> profile_keys
```

Source schema:

```text
2
```

Projection:

```text
payload = AdaAccessConfiguration
dependency = exact Profiles ProjectionTarget
```

ADA Access valida las profile keys contra el `ProfileCatalog` de esa dependencia.

Persistencia CURRENT:

```text
ProjectionRecord[AdaAccessConfiguration]
exact recursive dependencies
local provider
Cosmos provider
```

No reconstruir provenance desde la Profiles Projection CURRENT después de restart.

## Navigation CURRENT

```text
Navigation Configuration -> Profiles core
CURRENT

Navigation Configuration -> Profiles Configuration
FORBIDDEN

Navigation -> Users
FORBIDDEN

Navigation -> ADA Access
FORBIDDEN
```

Durable:

```text
NavigationLinkConfiguration.allowed_profiles
= tuple[str, ...] profile keys
```

## Manager / composition CURRENT

Manager core sigue generic.

ADA Configuration Manager CURRENT compone:

```text
Profiles
Navigation
Tools
KPI Configuration
KPI Definition
```

Profiles usa `web/compositions/profiles-manager`.

Users no es Source/Projection Manager module y su superficie administrativa sigue pendiente.

ADA Access tiene Source/Projection y persistencia CURRENT; su UI/composition administrativa
sigue pendiente.

## UI composition boundary

```text
shared behavior only when truly transversal
presentation remains local to each module
```

No transferir ownership visual por simetría.

## Known consumer conflict

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No añadir compatibility alias.

## Reglas congeladas

```text
Atlanticus generic
REQUIRED

Users -> profile_key
CURRENT

Users app-specific access state
FORBIDDEN

authority_key
REMOVED

local
LOCAL-RUNTIME ONLY

Users Configuration Source
REMOVED

Users generic Projection
REMOVED

Users Manager Source/Projection module
REMOVED

Profiles
GENERIC ATLANTICUS FIRST-CLASS CAPABILITY

Profiles -> ADA Configuration Manager
CURRENT VIA EXISTING PROFILES MANAGER COMPOSITION

ADA Access
APPLICATION-SPECIFIC

ADA Access user_id -> profile_keys
REMOVED

ADA Access Projection provenance
EXACT / DURABLE

Navigation authorization input
PROFILE KEY

Navigation Configuration -> Profiles core
CURRENT

Navigation dependency on Users
FORBIDDEN

Navigation dependency on ADA Access
FORBIDDEN

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN
```

## Pendientes explícitos y orden

```text
USERS-ADMINISTRATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED / AFTER USERS

MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED / AFTER USERS + ADA ACCESS

ADA Access runtime composition
PLANNED / SEPARATE

Navigation operational authorization alignment
PLANNED / SEPARATE

Navigation disabled-route surface
PLANNED / SEPARATE

concrete Entra/Graph provider
UNVERIFIED

Python metadata alignment
PLANNED / SEPARATE

WEB-TEST-CONTRACT-CLEANUP
PLANNED / SEPARATE

CI remote / full Ruff / full workspace qualification
UNVERIFIED
```
