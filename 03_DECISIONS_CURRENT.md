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

## ADA Access

CURRENT/FROZEN:

```text
AdaAccessConfiguration
├── access_keys
└── profile_access
```

La identidad durable de un acceso continúa siendo un único `access_key`.

La UI de creación compone:

```text
ámbito + permiso
→ ámbito.permiso
```

Eso no crea entidades durables separadas `scope` o `permission`.

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

No persistir grants redundantes para `root` o `local`.

La UI de Access puede usar `ProfileCatalog()` como fallback de sistema cuando no existe una
Profiles Projection activa. Al excluir `root` y `local`, `basic` y `guest` permanecen
asignables.

La validación del draft continúa requiriendo una Profiles Projection activa; el fallback de UI
no reemplaza esa dependencia de validación.

## ADA Access UI

CURRENT/FROZEN para el slice cerrado:

```text
tabs
Accesos / Perfiles

pagination
10 / 20, mediante atlanticus.web.pagination

profile row
summary + Configurar

inline growing multiselect
REMOVED

empty assignment modal
FORBIDDEN

profile assignment editor
viewport-centered modal

checkbox implementation
dbc.Checkbox

assignment state
single editable AdaAccessConfiguration

generic overflow hiding
FORBIDDEN

horizontal overflow
fix cause locally
```

La reserva vertical paginada se mantiene consistente con las surfaces ya corregidas.

El título visible `Perfiles` se localiza en la composition ADA; no se cambia el default generic
de `compose_profiles_manager`.

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

SUPERSEDED:

```text
Navigation Configuration -> Profiles core
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

Navigation generic does not know `basic`, `root`, `guest` or `local` as special keys.

ADA composition currently filters `root` and `local` from assignable Navigation profile
options.

## Navigation UI decisions

CURRENT/FROZEN:

```text
separate profiles context card
REMOVED

profiles
EDITED ONLY INSIDE LINK EDITOR

guest auto-selection
REMOVED

pagination
TOP-LEVEL NODES ONLY

section
COUNTS AS ONE TOP-LEVEL ITEM

section children
DO NOT COUNT TOWARD PAGE TOTAL

expanded section
SHOW ALL CHILDREN

page sizes
10 / 20

expanded state
EPHEMERAL / NOT SOURCE

horizontal overflow
FIX CAUSE; DO NOT HIDE GENERICALLY
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
```

Siguiente slice acordado:

```text
Perfiles
```

`Herramienta` sigue OPEN / DEFERRED; no está cerrada ni superseded.

Do not mix persistence qualification into UI review.

Do not modify code merely to satisfy tests that freeze CSS, markup, internal classes/functions
or visual structure.

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
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS / NEXT PAGE: PERFILES
```
