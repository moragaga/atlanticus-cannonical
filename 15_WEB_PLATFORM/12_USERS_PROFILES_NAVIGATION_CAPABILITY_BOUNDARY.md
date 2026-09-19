# Web Platform — Users / Profiles / Access / Navigation Capability Boundary

Estado: **CURRENT DECISION / REFINED AFTER GENERIC PAGINATION CUTOVER**

## Propósito

Fijar frontera CURRENT entre:

```text
Global Users
Generic Profiles
Application-specific ADA Access
Generic Navigation
Generic Web pagination behavior
Manager administrative shell
```

sin reintroducir Users Configuration Source, estado app-specific en Global User, Access
generic no demostrado, catálogo paralelo de Profiles, permissions internas de Manager
modeladas como perfiles ni presentación UI compartida por simetría.

## Autoridad de implementación

```text
moragaga/atlanticus@fbef06a8a0a587571527d9ecf131c73c5fc5f01a
```

Parent:

```text
9f12c41a23d69784c7c5b775a4093a94ac654d55
```

Tree:

```text
fc8c293f617aca4a53d89f687a22728e9d0fdcca
```

## Estado del frente

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
CLOSED / VERIFIED / CURRENT

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
CLOSED / VERIFIED / CURRENT

USERS-PERSISTED-DATA-CUTOVER
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILES-CONFIGURATION
CLOSED / VERIFIED / CURRENT

NONPROMOTED-ACCESS-SEMANTICS-CORRECTION
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT

MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT
CLOSED / VERIFIED / CURRENT

CONFIGURATION-UI-COMPOSITION-RECOVERY
CLOSED / VERIFIED / CURRENT

GENERIC-WEB-PAGINATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Regla principal

### Global Users

```text
identity + lifecycle + global base authority
```

No contiene profile/access app-specific.

### Profiles

```text
profile definition + catalog + configuration + Source lifecycle
```

Generic y reusable.

### ADA Access

```text
user_id -> profile_keys
profile_key -> ADA access_keys
```

Application-specific.

### Navigation

```text
allowed_profiles = profile keys
```

Generic. No importa Users ni ADA Access para resolver rutas.

### Generic Web pagination

```text
page state + slicing behavior
```

Generic Atlanticus.

No posee presentación visual.

### Manager authorization

```text
ManagerModule.access_key
principal.access_keys
```

Es una frontera funcional administrativa del Manager/composition y no cambia ownership de
Users, Profiles, ADA Access o Navigation.

## Users CURRENT

Managed authorities:

```text
basic
root
```

Runtime local authority:

```text
local
```

No Users authorities:

```text
guest
administrator
```

Login read-only; not promoted no bloquea entrada; disabled -> 403.

Administration core existe y UI sigue pendiente.

## Profiles CURRENT

```text
web/capabilities/profiles/core
web/capabilities/profiles/configuration
```

`ProfileDefinition`:

```text
key
label
background_color
text_color
```

Source lifecycle CURRENT.

No UI administrativa CURRENT.

Siguiente foco:

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
PLANNED / NEXT
```

## Generic pagination CURRENT

```text
web/framework/core/src/atlanticus/web/pagination.py
```

Contrato:

```text
DEFAULT_PAGE_SIZE = 10
ALLOWED_PAGE_SIZES = (10, 20)
PageRequest
Page
paginate_items
```

`Page` contiene sólo registros reales.

Los placeholders visuales necesarios para estabilizar una tabla son responsabilidad de la
presentación concreta.

No pertenecen al contrato:

```text
SortDirection
search/filter
Dash components
CSS
row placeholders
responsive layout
```

`ada.web.configuration.pagination` está REMOVED.

## ADA Access CURRENT

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration
```

Contracts:

```text
UserProfileAssignment
ProfileAccessGrant
EffectiveAdaAccess
AdaAccessConfiguration
```

Source lifecycle CURRENT.

No UI administrativa CURRENT.
No Projection CURRENT.
Runtime composition exacta sigue separada/open.

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

## Manager / application composition CURRENT

ADA Configuration Manager compone:

```text
Navigation
Tools
KPI Configuration
KPI Definition
```

Manager access:

```text
navigation.manage
tools.manage
kpis.manage
```

No bypass por `is_local` ni `administrator`.

Users no vuelve a ser Manager Source/Projection module.

Profiles Configuration y ADA Access Configuration pueden recibir superficies administrativas
futuras sin cambiar ownership de dominio.

## UI composition boundary

Regla congelada:

```text
shared behavior only when truly transversal
presentation remains local to each module
```

Por tanto:

```text
Profiles UI != KPI UI component reuse by default
Users UI != Profiles UI component reuse by default
ADA Access UI != Profiles UI component reuse by default
```

Uniformidad visual se logra siguiendo patrones/tokens vigentes, no transfiriendo ownership
de la presentación.

Paginación es el ejemplo CURRENT de frontera transversal válida: se comparte el cálculo,
no el markup/CSS.

## Known consumer conflict

`web/compositions/navigation-manager` usa `can_access(...)` aunque
`ManagerAuthorizationPolicy` CURRENT expone `can_view(...)`.

Debe alinearse directamente cuando entre al scope.

## Reglas congeladas

```text
Atlanticus generic
REQUIRED

Global Users standalone
REQUIRED

Global Users app-specific state
FORBIDDEN

Managed authority
basic | root

local
LOCAL-RUNTIME ONLY

administrator
REMOVED FROM USERS

guest
REMOVED FROM USERS AUTHORITY CONTRACT

Users Configuration Source
REMOVED

Users generic Projection
REMOVED

Users Manager Source/Projection module
REMOVED

Users login write/pending
FORBIDDEN

Profiles
GENERIC ATLANTICUS FIRST-CLASS CAPABILITY

ADA Access
APPLICATION-SPECIFIC / CURRENT

Navigation authorization input
PROFILE KEY

Navigation Configuration -> Profiles core
CURRENT

Navigation dependency on Users
FORBIDDEN

Navigation dependency on ADA Access
FORBIDDEN

Generic pagination behavior
ATLANTICUS OWNED

Pagination presentation
UI OWNED / NOT GENERIC BY DEFAULT

Page sizes
10 | 20

Manager authorization
EXPLICIT MODULE ACCESS KEY

Manager is_local/admin implicit bypass
FORBIDDEN

Manager per-operation permission split
REMOVED

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN
```

## Pendientes explícitos

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
PLANNED / NEXT

PROFILES-CONFIGURATION-WEB-SURFACE
PLANNED

NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT

Users Administration UI
PLANNED

ADA Access Configuration UI
PLANNED

MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED / FINAL

exact guest fallback composition
PLANNED / SEPARATE

ADA Access runtime composition
PLANNED / SEPARATE

concrete Entra/Graph provider
UNVERIFIED

Python metadata alignment
PLANNED / SEPARATE

WEB-TEST-CONTRACT-CLEANUP
PLANNED / SEPARATE

CI remote / full Ruff
UNVERIFIED
```
