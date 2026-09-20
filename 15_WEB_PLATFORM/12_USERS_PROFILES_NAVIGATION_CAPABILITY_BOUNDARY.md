# Web Platform — Users / Profiles / Access / Navigation Capability Boundary

Estado: **CURRENT DECISION / REFINED AFTER ADA ACCESS MANAGER INTEGRATION**

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
moragaga/atlanticus@6032cf84e8a5ad1f7a4cde4333513a04bcdd659a
```

Parent inmediato verificado:

```text
783d3578da52aeb5cf831999a7717dc8b79f2fb0
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

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-CHECKLIST-COMPATIBILITY
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

MANAGER-FINAL-ADMIN-COMPOSITION
CLOSED / VERIFIED / CURRENT

MANAGER-ALL-SURFACES-RENDERABLE
CLOSED / VERIFIED MANUAL / CURRENT

MANAGER-UI-CONSISTENCY-REVIEW
PLANNED / NEXT

MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW
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
declared access_keys
profile_key -> access_keys
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

Lifecycle administrativo:

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

Users Manager CURRENT:

```text
web/compositions/users-manager
ManagerEntry
users.manage
/manager/users
```

La Web surface permite candidate/promote y promoted/edit.

Sólo administra:

```text
profile_key
enabled
```

Identity/directory fields permanecen read-only.

Profiles options se congelan al cargar la página y sólo se reemplazan con Refresh explícito.

El fix CURRENT para dbc 2.0.4 mantiene el mismo comportamiento y sólo corrige el render del
Checklist de `enabled`.

No existe CURRENT:

```text
authority_key
User authority basic|root mini-contract
administrator -> root alias
Users Manager Source/Projection module
Users Configuration Source
Users generic Projection
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

Profiles Manager es CURRENT y ADA Configuration Manager consume su `ManagerModule`.

## ADA Access CURRENT

Packages:

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration
scopes/ada/web/access/projection-local
scopes/ada/web/access/projection-cosmos
```

Ownership:

```text
profile_key -> access_keys
```

Contracts:

```text
ProfileAccessGrant
EffectiveAdaAccess(profile_key, access_keys)

AdaAccessConfiguration
├── access_keys
└── profile_access
```

`access_key` es la identidad estable.

No existe CURRENT:

```text
UserProfileAssignment
user_id -> profile_keys
AccessDefinition entity paralela
access_id
permission_id
```

Las access keys se normalizan y son únicas.

Cada grant sólo puede referenciar access keys declaradas.

Una access key asignada debe desasignarse antes de eliminarla.

Source:

```text
ADA_ACCESS_SOURCE_SCHEMA_VERSION = 3
```

No existe decoder de compatibilidad para schemas anteriores.

Projection durable:

```text
ADA_ACCESS_PROJECTION_SCHEMA_VERSION = 2
payload = AdaAccessConfiguration
dependency = exact Profiles ProjectionTarget
```

Persistencia:

```text
ProjectionRecord[AdaAccessConfiguration]
exact recursive dependencies
local provider
Cosmos provider
```

No reconstruir provenance desde la Profiles Projection CURRENT después de restart.

## ADA Access Web / Manager CURRENT

La Web surface está implementada en el package existente de Access Configuration.

No se creó una capability generic nueva ni una composition Atlanticus genérica para Access.

Manager:

```text
ManagerModule
key = access
title = Accesos
group = configuration
route = /access
effective route = /manager/access
order = 15
access_key = access.manage
```

La UI:

```text
define access key
remove unassigned access key
assign declared access keys to projected Profiles
save browser draft through Manager workspace
```

Los Profiles disponibles provienen de la Profiles Projection.

La draft validation requiere Profiles Projection y valida referencias contra su
`ProfileCatalog`.

No existe asociación durable:

```text
user -> access_keys
```

Users conserva únicamente:

```text
user -> profile_key
```

La futura resolución runtime prevista sigue la frontera:

```text
authenticated user
→ Users profile_key
→ ADA Access projection
→ EffectiveAdaAccess/access_keys
→ runtime/session/request memory
→ Web consumers
```

Ese runtime no fue implementado en este incremento.

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

Tipos CURRENT:

```text
ManagerModule
ManagerEntry
```

ADA Configuration Manager compone:

```text
Administración:
- Users

Configuraciones:
- Profiles
- Accesos
- Navegación
- Herramienta
- KPI
- Definiciones KPI
```

Profiles usa `web/compositions/profiles-manager`.

Users usa `web/compositions/users-manager`.

Access usa su Source/Projection real y un `ManagerModule` application-specific construido
dentro de ADA Configuration Manager.

Users no es Source/Projection Manager module.

Todas las superficies fueron observadas como visibles/renderizables durante el cierre.

## UI composition boundary

```text
shared behavior only when truly transversal
presentation remains local to each capability
```

No transferir ownership visual por simetría.

La siguiente etapa puede corregir presentación y paginación visual, pero no debe crear
arquitectura nueva para homogeneizar UI.

## Testing boundary congelado

Durante UI review:

```text
behavior tests
KEEP

CSS/style/visual structure tests
REMOVE

JS internal structure/content tests
REMOVE

tests for existence/non-existence of internal classes/functions
REMOVE
```

Assets sólo pueden comprobarse como presentes/cargables si su carga es contractualmente
relevante.

Paginación funcional sí puede probarse; la forma visual de la paginación se valida
visualmente.

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

Users ManagerEntry
CURRENT

Profiles
GENERIC ATLANTICUS FIRST-CLASS CAPABILITY

Profiles -> ADA Configuration Manager
CURRENT VIA EXISTING PROFILES MANAGER COMPOSITION

ADA Access
APPLICATION-SPECIFIC

ADA Access access_keys catalog
CURRENT

ADA Access profile_key -> access_keys
CURRENT

ADA Access user_id -> profile_keys
REMOVED

ADA Access user durable access_keys
FORBIDDEN

ADA Access Projection provenance
EXACT / DURABLE

ADA Access Web surface
CURRENT

ADA Access Manager integration
CURRENT

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN

LEGACY SCHEMA READERS
FORBIDDEN
```

## Pendientes explícitos y orden

```text
MANAGER-UI-CONSISTENCY-REVIEW
PLANNED / NEXT

WEB-TEST-CONTRACT-CLEANUP
PLANNED / NEXT / COUPLED TO UI REVIEW

MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW

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

CI remote / full global qualification
UNVERIFIED
```
