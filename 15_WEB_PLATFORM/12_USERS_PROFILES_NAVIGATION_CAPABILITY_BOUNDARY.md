# Web Platform — Users / Profiles / Access / Navigation Capability Boundary

Estado: **CURRENT DECISION / REFINED AFTER NAVIGATION STANDALONE CUTOVER**

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
moragaga/atlanticus@29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
```

Parent:

```text
856498c52f182cd531deae845c25bd51ae2ff4ea
```

## Estado del frente

```text
PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

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
IN PROGRESS

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

SUPERSEDED:

```text
Navigation Configuration -> Profiles core
```

CURRENT:

```text
Navigation Configuration
independent from Profiles / Users / ADA

NavigationProfileOption
key + label

NavigationProfileOptionsProvider
optional
```

El provider opcional es un contract neutral de Navigation.

Una application/composition que conoce Profiles puede adaptar:

```text
ProfileCatalog
→ tuple[NavigationProfileOption, ...]
```

Si no hay provider:

```text
Navigation Configuration sigue siendo operable
```

Si hay provider:

```text
se puede validar que allowed_profiles sólo use keys conocidas
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

## Navigation Configuration UI CURRENT

```text
standalone profiles card
REMOVED

profiles
link editor only

guest implicit selection
REMOVED

empty allowed profiles label
Acceso: Público
```

Paginación:

```text
top-level nodes only
page size 10 / 20
sections count as one
children do not count
expanded section shows all children
multiple sections may remain expanded
expanded state is ephemeral
```

El empty state conserva altura de página y centra su contenido.

La corrección de overflow horizontal está contenida en el adapter Dash de Navigation; no se
usa un hide global como sustituto.

## Manager / composition CURRENT

Manager core sigue generic.

ADA Configuration Manager compone:

```text
Administración:
- Users

Configuraciones:
- Profiles
- Access
- Navigation
- Tools
- KPI
- KPI Definition
```

La UI específica permanece en cada capability.

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

Profiles
GENERIC ATLANTICUS FIRST-CLASS CAPABILITY

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
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS / NEXT PAGE: HERRAMIENTA

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
