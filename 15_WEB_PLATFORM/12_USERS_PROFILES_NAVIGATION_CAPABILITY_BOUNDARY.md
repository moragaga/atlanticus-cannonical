# Web Platform — Users / Profiles / Access / Navigation Capability Boundary

Estado: **CURRENT DECISION / REFINED AFTER PROFILES UI CLOSURE**

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
moragaga/atlanticus@df5b99502265758e873e0565abf2176cc617104b
```

Parent:

```text
31723a108ddd2f49346fdcbb844db9891eb08f4b
```

Tree:

```text
de1151ba72d44bc8ac6b6f2cfd6f57eb7e80c0a0
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
IN PROGRESS / NEXT PAGE: USERS

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

Managed profiles usan `ProfileCatalog`.

`local` permanece runtime-only y no es managed assignment.

Users Administration CURRENT:

```text
UsersAdministrationService
├── discover
├── promote
└── update
```

Manager integration:

```text
Users
→ ManagerEntry
→ no synthetic Source/Projection
```

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

El cierre visual CURRENT no cambia esa frontera.

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

Assignable:

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

Esta exclusión pertenece a la composition ADA.

Navigation generic no contiene lógica especial para `root` ni `local`.

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

Profiles composition puede recibir metadata visible de runtime sin transferir ownership a
Manager.

## Testing boundary

KEEP:

```text
behavior tests
boundary/import tests reales
functional pagination tests
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
USERS-MANAGER-UI-REVIEW
PLANNED / NEXT

MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS / NEXT PAGE: USERS

Herramienta final visual consistency
OPEN / DEFERRED

MANAGER-RESPONSIVE-MEDIA-QUERY-AUDIT
PLANNED / PHASE 2

WEB-TEST-CONTRACT-CLEANUP
PLANNED / PHASE 3

MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW

Navigation operational authorization alignment
BLOCKED / SEPARATE

concrete Entra/Graph provider
UNVERIFIED

Python metadata alignment
PLANNED / SEPARATE

CI remote / full global qualification
UNVERIFIED
```
