# Web Platform — Users / Profiles / Access / Navigation Capability Boundary

Estado: **CURRENT DECISION / REFINED AFTER USERS UI CLOSURE**

## Propósito

Fijar la frontera CURRENT entre:

```text
Global Users
Generic Profiles
Application-specific ADA Access
Generic Navigation
Manager administrative shell
```

sin reintroducir estado app-specific en Users, catálogos paralelos, hard dependencies
innecesarias ni contracts legacy.

## Autoridad de implementación

```text
moragaga/atlanticus@ce07ada07e3f4f100b97ad2ac5e7285b54419c20
```

Parent:

```text
df5b99502265758e873e0565abf2176cc617104b
```

Tree:

```text
825dbaffba30b42199c54dbd4da9ba234f3ef437
```

## Estado del frente

```text
PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-UI-REVIEW
CLOSED / CURRENT / ACCEPTED WITH NON-BLOCKING POLISH

USERS-GUEST-ASSIGNMENT-BOUNDARY
CURRENT / IMPLEMENTED

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ACCESS-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT

MANAGER-FINAL-ADMIN-COMPOSITION
CLOSED / VERIFIED / CURRENT

NAVIGATION-STANDALONE-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-PUBLIC-ACCESS-CONTRACT
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILE-OPTIONS-DECOUPLING
CLOSED / VERIFIED / CURRENT

NAVIGATION-CONFIGURATION-UI-PASS
CLOSED / VERIFIED MANUAL / CURRENT

MANAGER-UI-CONSISTENCY-REVIEW
CLOSED FOR CURRENT V1 / NON-BLOCKING POLISH DEFERRED

MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / OPEN
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
navigation structure + route visibility by profile keys
```

Generic Atlanticus.

Navigation core/configuration no depende de Users ni ADA Access.

Navigation Configuration tampoco depende de Profiles.

## Users CURRENT

Contrato durable/effective:

```text
UserRecord.profile_key
EffectiveUser.profile_key
```

Profiles posee el catálogo; Users decide qué profiles son asignables a usuarios administrados.

### Profile states

System profiles CURRENT:

```text
basic
root
guest
local
```

Semántica Users:

```text
basic
assignable managed profile

root
assignable managed profile

configured custom profile
assignable managed profile

guest
valid transient/pending profile
NOT administratively assignable

local
runtime-local only
NOT administratively assignable
```

La distinción es contractual:

```text
normalize_managed_profile_key('guest')
VALID

UserRecord(profile_key='guest')
VALID

require_managed_profile('guest', ...)
REJECT

available_managed_profiles(...)
EXCLUDES guest + local
```

No mover la prohibición de `guest` al constructor de `UserRecord`: los registros pendientes pueden
necesitar representar ese estado transitorio antes de promoción.

Users Administration CURRENT:

```text
UsersAdministrationService
├── discover
├── promote
└── update
```

Promoción V1:

```text
one user at a time
explicit Promover action
no batch contract
```

Update V1:

```text
identity
informational / immutable from Users Administration

profile + enabled
editable

Guardar
explicit immediate commit
```

No existe borrador global de Users.

### Persistence sequence

Contrato implementado:

```text
promote
UsersRegistryStore.replace(...)
→ UsersAdministrationStore.create(...)

update
UsersRegistryStore.replace(...)
→ UsersAdministrationStore.replace(...)
```

La secuencia lógica está implementada.

El wiring productivo concreto de adapters Blob/Cosmos permanece:

```text
UNVERIFIED IN THIS CLOSURE
```

No confundir la prueba in-process usada para UI con una qualification Azure end-to-end.

### Manager integration

```text
Users
→ ManagerEntry
→ no synthetic Source/Projection
```

## Users UI CURRENT

La capability Users posee su UI.

Composición CURRENT:

```text
Control de usuarios
├── Usuarios
└── Por promover
```

Vista default:

```text
Usuarios
```

Sólo una vista se muestra como activa a la vez.

Paginación:

```text
atlanticus.web.pagination
page sizes 10 / 20
numbered navigation
stable result viewport
```

Tabs:

```text
existing Atlanticus convention
transparent inactive
gold underline active / hover / focus
```

Editor:

```text
capability-local viewport modal
backdrop
close / cancel / save
```

No usar `dbc.Modal` como owner del editor de Users si saca el contenido de la frontera visual de la
capability.

Los detalles visuales menores pendientes son no bloqueantes y quedan deferred; no alteran estos
contratos.

## Profiles CURRENT

System profiles:

```text
basic
root
guest
local
```

Profiles posee el catálogo.

Eso no obliga a cada consumer a exponer todos los profiles como opciones de UI.

Profiles UI CURRENT:

```text
normal profile visual
one uppercase initial

local visual
local identities
first + last initials

source/projection labels
composition-driven

profile editor
capability-local viewport modal

pagination
10 / 20 via atlanticus.web.pagination
```

`local` como excepción visual no crea un nuevo dominio de profile identities.

## ADA Access CURRENT

Ownership:

```text
profile_key -> access_keys
```

No existe durable:

```text
user -> access_keys
```

## Navigation CURRENT

CURRENT:

```text
Navigation Configuration
independent from Profiles / Users / ADA

NavigationProfileOption
key + label

NavigationProfileOptionsProvider
optional
```

Una application/composition que conoce Profiles puede adaptar:

```text
ProfileCatalog
→ tuple[NavigationProfileOption, ...]
```

## Navigation access semantics CURRENT

```text
enabled = False
→ deny

enabled = True
allowed_profiles = ()
→ public within Navigation authorization

enabled = True
allowed_profiles = non-empty
→ require profile/access key membership
```

`principal.unrestricted` evita restricciones por profile, pero no habilita un route disabled.

## ADA Navigation profile options

ADA Configuration Manager actualmente adapta Profiles para Navigation.

Assignable en Navigation Configuration:

```text
basic
guest
custom configured profiles
```

No assignable en esa UI:

```text
root
local
```

Esto no contradice la frontera Users:

```text
guest
puede participar en semántica de visibilidad/navigation

guest
no puede convertirse en perfil final asignado por Users Administration
```

La exclusión/selección de Navigation pertenece a la composition ADA.

Navigation generic no contiene lógica especial para `root`, `guest` ni `local`.

## Manager / composition CURRENT

Manager core sigue generic.

ADA Configuration Manager compone:

```text
Administración:
- Users

Configuraciones:
- Perfiles
- Accesos
- Navigation
- Tools
- KPI
- KPI Definition
```

La UI específica permanece en cada capability.

Profiles composition puede recibir metadata visible de runtime sin transferir ownership a Manager.

## Testing boundary

KEEP:

```text
behavior tests
domain invariant tests
boundary/import tests reales
functional pagination tests
promotion/update tests
persistence/recovery tests where contractually relevant
```

REMOVE / DO NOT ADD:

```text
CSS visual tests
responsive visual tests
overflow visual tests
markup-shape tests without behavior
tests for internal class/function existence
tests for JS internal structure
```

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

Users Manager integration
ManagerEntry

Users synthetic Manager Source/Projection
FORBIDDEN

Users global draft/publication workflow
FORBIDDEN

Users promotion V1
ONE USER AT A TIME

Users identity editing
FORBIDDEN

guest pending/transient representation
ALLOWED

guest managed assignment
FORBIDDEN

local managed assignment
FORBIDDEN

Profiles
GENERIC ATLANTICUS FIRST-CLASS CAPABILITY

Profiles UI ownership
PROFILES CAPABILITY

ADA Access
APPLICATION-SPECIFIC

Navigation Configuration -> Profiles core
REMOVED

Navigation neutral profile options provider
CURRENT / OPTIONAL

Navigation -> Users
FORBIDDEN

Navigation -> ADA Access
FORBIDDEN

empty allowed_profiles
PUBLIC WITHIN NAVIGATION AUTHORIZATION

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN

LEGACY SCHEMA READERS
FORBIDDEN
```

## Pendientes explícitos

```text
Users final targeted/full automated requalification
UNVERIFIED

Users residual visual polish
PLANNED / DEFERRED / NON-BLOCKING

MANAGER-RESPONSIVE-MEDIA-QUERY-AUDIT
PLANNED / DEFERRED

WEB-TEST-CONTRACT-CLEANUP
PLANNED / DEFERRED

MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / OPEN

Navigation operational authorization alignment
BLOCKED / SEPARATE

concrete Entra/Graph provider
UNVERIFIED

Python metadata alignment
PLANNED / SEPARATE

CI remote / full global qualification
UNVERIFIED
```
