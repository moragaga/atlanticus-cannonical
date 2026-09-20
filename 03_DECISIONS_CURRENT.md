# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / LOCALLY USED / METADATA NOT YET GLOBALLY ALIGNED |
| `python:3.14.7-slim-trixie` | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `uv`, no pip normal | CURRENT |
| Definir contratos antes que consumidores | CURRENT |
| Backend antes que frontend | CURRENT |
| Cutover raíz limpio | CURRENT |
| No shims/adapters/aliases legacy | FROZEN |
| No doble contrato | FROZEN |
| Tests no son autoridad sobre contracts SUPERSEDED | FROZEN |
| Presentación propia por módulo; reutilizar sólo comportamiento transversal real | FROZEN |
| Un foco por incremento | FROZEN |

## Regla universal de cutover

```text
LEGACY
REMOVE

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN

OLD SCHEMA READERS IN CURRENT RUNTIME
FORBIDDEN
```

## Generic Web pagination

CURRENT/FROZEN:

```text
atlanticus.web.pagination
DEFAULT_PAGE_SIZE = 10
ALLOWED_PAGE_SIZES = (10, 20)
PageRequest
Page
paginate_items(...)
```

Presentación/CSS/placeholders/search/filter/sort no pertenecen al contrato generic.

## Manager

CURRENT:

```text
ManagerModule
ManagerEntry
ManagerAuthorizationPolicy.can_view(principal, item)
```

No bypass por `is_local` ni profile administrator.

## Users / Profiles

CURRENT:

```text
Profiles
owns profile definitions/catalog

Users
owns user -> profile_key
```

SUPERSEDED / REMOVED:

```text
authority_key
basic|root authority mini-contract
administrator/root aliases
```

Managed users consumen `ProfileCatalog`; `local` no es managed assignment.

Users Administration permanece `ManagerEntry`; no crear Source/Projection artificial para
alinearlo visualmente con `ManagerModule`.

## Profiles UI CURRENT

El cierre visual de Profiles no cambia el dominio durable.

CURRENT/FROZEN para la surface cerrada:

```text
source/projection labels
composition-driven

pagination
10 / 20 via atlanticus.web.pagination

configured profile avatar
single uppercase initial

local profile presentation
special local identity list

local identity avatar
first + last initials, uppercase

profile editor
viewport modal owned by Profiles

color feedback
live preview + current hex values

footer empty spacing
removed
```

La excepción visual de `local` no crea un nuevo profile contract ni un estado durable por
identidad local.

`compose_profiles_manager(...)` puede recibir `description`, `source_name` y
`projection_name`; sus defaults siguen siendo generic. ADA localiza título/descripción y
nombres de runtime desde la composition.

SUPERSEDED dentro de la UI Profiles:

```text
dbc.Modal dependency for profile editor
SUPERSEDED / REMOVED

previous/next-only profile pagination presentation
SUPERSEDED

single Source-only runtime context
SUPERSEDED

Local as one fixed-color visual badge
SUPERSEDED
```

## ADA Access

CURRENT/FROZEN:

```text
AdaAccessConfiguration
├── access_keys
└── profile_access
```

Profiles irrestrictos CURRENT:

```text
root
local
```

Semántica:

```text
resolve(root|local)
→ todos los access_keys definidos

explicit grant root|local
→ reject
```

Los demás profiles usan grants explícitos.

## Navigation standalone boundary

CURRENT/FROZEN:

```text
Navigation Configuration
MUST NOT depend on Profiles, Users or ADA

NavigationProfileOption
NavigationProfileOptionsProvider
owned by Navigation Configuration

provider
OPTIONAL

external profile catalog adaptation
belongs to application/composition boundary
```

Navigation durable access rule:

```text
allowed_profiles = ()
PUBLIC WITHIN NAVIGATION AUTHORIZATION

allowed_profiles = non-empty
RESTRICTED TO PROFILE KEYS

enabled = False
DENY

principal.unrestricted
PROFILE-RESTRICTION BYPASS ONLY
```

## Manager UI qualification order

CURRENT decision:

```text
1. desktop/page visual consistency
2. responsive/media-query audit
3. behavior-focused test qualification and invalid-test cleanup
```

Slices CURRENT ya cerrados:

```text
Navigation
Accesos
Perfiles
```

Siguiente slice acordado:

```text
Users
```

`Herramienta` sigue OPEN / DEFERRED; no está cerrada ni superseded.

El usuario indicó que después de Users se puede cerrar el trabajo actual de Manager por ahora.
Ese cierre no convierte frentes diferidos en VERIFIED.

Do not mix persistence qualification into UI review.

## Testing

Automatizar:

```text
behavior
contracts
invariants
regressions
critical callbacks
functional pagination
```

Visual/manual:

```text
CSS
spacing
branding
responsive
overflow
alignment
visual pagination form
```

## Conflict CURRENT conocido

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`ManagerAuthorizationPolicy` expone `can_view(...)`.

`web/compositions/navigation-manager` continúa invocando `can_access(...)`.

No crear alias para conservar el consumer.

## Siguiente foco

```text
USERS-MANAGER-UI-REVIEW
PLANNED / NEXT
```
