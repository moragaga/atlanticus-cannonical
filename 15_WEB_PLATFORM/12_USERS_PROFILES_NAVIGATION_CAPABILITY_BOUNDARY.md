# Web Platform — Users / Profiles / Access / Navigation Capability Boundary

Estado: **CURRENT DECISION / REFINED AFTER MANAGER AUTHORIZATION ALIGNMENT**

## Propósito

Fijar frontera CURRENT entre:

```text
Global Users
Generic Profiles
Application-specific ADA Access
Generic Navigation
Manager administrative shell
```

sin reintroducir Users Configuration Source, estado app-specific en Global User, Access
generic no demostrado, catálogo paralelo de Profiles ni permissions internas de Manager
modeladas como perfiles.

## Autoridad de implementación

```text
moragaga/atlanticus@9f12c41a23d69784c7c5b775a4093a94ac654d55
```

Parent:

```text
3eb46dac80f23d438774e3afa39999dc96f592d7
```

Tree:

```text
dd002b632b494065428af9dd10f1e58b7e6638d1
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

### Manager authorization

```text
ManagerModule.access_key
principal.access_keys
```

Es una frontera funcional administrativa del Manager/composition y no cambia ownership de
Users, Profiles, ADA Access o Navigation.

No interpretar:

```text
navigation.manage
tools.manage
kpis.manage
```

como Profiles ni como Users authorities.

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

`NavigationProfileCatalogProvider` consume `ProfileCatalog` opcionalmente.
No inventa profiles base.

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

Users Administration puede convivir en una aplicación administrativa, pero debe consumir
su lifecycle propio en lugar de fingir Source/Projection.

## UI recovery boundary

Ausencia CURRENT verificada:

```text
Profiles Configuration UI
Users Administration UI
ADA Access Configuration UI
```

El usuario reporta que hubo composiciones/visualizaciones transversales anteriores que se
perdieron. Su existencia y shape exactos son históricos UNVERIFIED hasta localizar código o
evidencia concreta.

Regla para recovery:

```text
CURRENT contracts first
historical code second as reference
no memory-driven reconstruction
no legacy resurrection
no adapters/shims/aliases
one UI increment at a time
```

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
CONFIGURATION-UI-COMPOSITION-RECOVERY
PLANNED / NEXT

NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT

Profiles Configuration UI
PLANNED

Users Administration UI
PLANNED

ADA Access Configuration UI
PLANNED

exact guest fallback composition
OPEN / SEPARATE

ADA Access runtime composition
OPEN / SEPARATE

concrete Entra/Graph provider
UNVERIFIED

Python metadata alignment
OPEN / SEPARATE

WEB-TEST-CONTRACT-CLEANUP
OPEN / SEPARATE

CI remote / full Ruff
UNVERIFIED
```
