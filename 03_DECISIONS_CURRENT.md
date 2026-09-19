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
| No crear shims/adapters/aliases temporales para legacy | FROZEN |
| No conservar doble contrato | FROZEN |
| Tests no son autoridad sobre contratos SUPERSEDED | FROZEN |
| Un consumer puede quedar temporalmente roto durante un root cutover | FROZEN |
| Presentación propia por módulo; reutilizar sólo comportamiento realmente transversal | FROZEN |

## Regla universal de cutover

```text
LEGACY
REMOVE

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOBLE CONTRATO
FORBIDDEN

OLD SCHEMA READERS IN CURRENT RUNTIME
FORBIDDEN

CONTRATO FINAL
Responsabilidad real del dominio; infraestructura genérica sólo donde aplique
```

No forzar Source/Projection/Manager sobre una entidad que no sea configuration lifecycle.

## Source / Projection

Permanecen CURRENT/FROZEN:

```text
Source generic -> web/capabilities/source
Projection exact-release -> web/capabilities/projection/core
release identity != content hash
ProjectionTarget = SourceKey + SourceReleaseRef + dependencies
project(target) no relee current
Manager no reconstruye ProjectionTarget desde revision
expected_source_revision REMOVED
```

## Generic Web pagination

Contrato CURRENT/FROZEN:

```text
atlanticus.web.pagination
├── DEFAULT_PAGE_SIZE = 10
├── ALLOWED_PAGE_SIZES = (10, 20)
├── PageRequest
├── Page
└── paginate_items(...)
```

Responsabilidad:

```text
page number / page size
range
page count
previous / next
clamp a página válida
slice de items reales
```

No responsabilidad:

```text
markup Dash
CSS
placeholders visuales
search/filter/sort
SortDirection
modal/table/card rendering
```

El contrato legacy `ada.web.configuration.pagination` está SUPERSEDED / REMOVED.

No crear alias de nombres anteriores:

```text
ConfigurationPageRequest
ConfigurationPage
DEFAULT_CONFIGURATION_PAGE_SIZE
ALLOWED_CONFIGURATION_PAGE_SIZES
```

La presentación ADA actual puede consumir el contrato generic sin transferirse a Atlanticus.

## UI ownership

Cada superficie mantiene presentación propia.

La reutilización transversal requiere comportamiento compartido real, no similitud visual.

Para superficies paginadas administrativas se conserva la decisión de interacción:

```text
default page size = 10
allowed page sizes = 10 | 20
```

Si una presentación necesita altura estable puede completar visualmente hasta `page_size`,
pero esos placeholders no pertenecen a `Page` ni a `paginate_items`.

## Manager generic contract

CURRENT:

```text
ManagerModule
├── source_key
├── source_service
├── source_reader_service
├── projection_service
├── draft_validation_service
├── source_history_service | None
└── access_key | None
```

SUPERSEDED / REMOVED:

```text
ManagerModuleAccess
workflow_service
exact_source_*
exact_projection_service
expected_source_revision
per-operation Manager access fields
```

### Manager authorization

Contrato CURRENT:

```text
ManagerAuthorizationPolicy.can_view(principal, module)
```

Política default:

```text
required = module.access_key
required is None -> deny
required in principal.access_keys -> allow
otherwise -> deny
```

No hay bypass de autorización por:

```text
is_local
administrator profile
```

## Global Users

Users es registry/lifecycle global y no Configuration Source.

Managed authority:

```text
basic
root
```

Runtime local:

```text
local
```

No son Users authority CURRENT:

```text
guest
administrator
```

Users Administration UI debe consumir el lifecycle existente; no reintroducir Users
Source/Projection ni Users Manager configuration module.

## Profiles

Profiles es capability generic Atlanticus first-class.

```text
profiles/core
→ ProfileDefinition
→ ProfileCatalog

profiles/configuration
→ ProfilesConfiguration
→ Profiles Source lifecycle
```

La futura UI consume estos contratos; no los redefine.

Profiles puede usar `atlanticus.web.pagination` directamente.

## ADA Access

ADA Access es application-specific.

```text
user_id -> profile_keys
profile_key -> access_keys
```

No convertir ADA Access en dependency de Navigation.

## Navigation / Profiles

CURRENT:

```text
Navigation Configuration -> Profiles core
Navigation durable allowed_profiles = profile keys
Navigation -> Profiles Configuration FORBIDDEN
Navigation -> Users FORBIDDEN
Navigation -> ADA Access FORBIDDEN
```

## UI composition boundary

El Manager genérico posee shell/home/sidebar/workflow administrativo.

La configuración concreta de cada dominio permanece en su capability/domain.

No crear un design system administrativo compartido sólo porque varias pantallas sean
tablas con paginación.

Estado CURRENT de superficies faltantes:

```text
Profiles Configuration UI
PLANNED

Users Administration UI
PLANNED

ADA Access Configuration UI
PLANNED
```

Secuencia vigente:

```text
1. PROFILES-CONFIGURATION-EDITOR-CONTRACT
2. PROFILES-CONFIGURATION-WEB-SURFACE
3. USERS-ADMINISTRATION-UI
4. ADA-ACCESS-CONFIGURATION-UI
5. MANAGER-FINAL-ADMIN-COMPOSITION
```

No mezclar esas superficies en un solo incremento.

## Testing

Automatizar:

```text
behavior
contracts
invariants
regressions
critical flows
```

No crear tests cuyo único objetivo sea fijar:

```text
CSS visual
clases CSS
responsive visual
spacing
branding
estructura HTML accidental
existencia/no existencia de funciones o clases
nombres privados
implementación interna
```

Assets JS/CSS sólo se automatizan cuando su disponibilidad es requisito contractual.

Responsive, densidad, spacing, branding y apariencia se validan visualmente salvo
comportamiento funcional automatizable.

## Conflict CURRENT conocido

`web/compositions/navigation-manager` usa `authorization.can_access(...)` aunque el
protocolo CURRENT sólo declara `can_view(...)`.

No crear alias `can_access` para conservar el consumer.

## Siguiente foco

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
PLANNED / NEXT
```

Definir primero el contrato del editor; Web surface después.
