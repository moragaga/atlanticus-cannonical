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
| Tests no son autoridad sobre contratos SUPERSEDED | FROZEN |
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

## Source / Projection

CURRENT/FROZEN:

```text
Source generic -> web/capabilities/source
Projection exact-release -> web/capabilities/projection/core
ProjectionTarget = SourceKey + SourceReleaseRef + dependencies
project(target) no relee current
Manager no reconstruye ProjectionTarget desde revision
expected_source_revision REMOVED
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
ManagerAuthorizationPolicy.can_view(principal, module)
```

No bypass por `is_local` ni profile administrator.

Profiles posee composition Manager reusable.

Users no es Source/Projection Manager module.

## Users / Profiles

Decisión CURRENT:

```text
Profiles
owns profile definitions/catalog

Users
owns user -> profile_key
```

Contrato:

```text
UserRecord.profile_key
EffectiveUser.profile_key
```

SUPERSEDED / REMOVED:

```text
authority_key
basic|root authority mini-contract
administrator/root aliases
```

Managed users consumen `ProfileCatalog`; `local` no es managed assignment.

## ADA Access

ADA Access es application-specific.

CURRENT:

```text
profile_key -> access_keys
```

SUPERSEDED / REMOVED:

```text
user_id -> profile_keys
UserProfileAssignment
```

Contracts CURRENT:

```text
ProfileAccessGrant
EffectiveAdaAccess(profile_key, access_keys)
AdaAccessConfiguration
```

Source schema CURRENT:

```text
2
```

Projection CURRENT:

```text
ProjectionRecord[AdaAccessConfiguration]
```

con dependencia exacta sobre Profiles Projection.

No crear `AdaAccessCatalog` paralelo sin una responsabilidad independiente demostrada.

## Navigation / Profiles

CURRENT:

```text
Navigation Configuration -> Profiles core
Navigation durable allowed_profiles = profile keys
Navigation -> Profiles Configuration FORBIDDEN
Navigation -> Users FORBIDDEN
Navigation -> ADA Access FORBIDDEN
```

## UI ownership

Cada superficie mantiene presentación propia.

CURRENT reusable:

```text
Profiles Configuration Web surface
Profiles Manager composition
```

Pendientes separados:

```text
Users Administration UI
ADA Access Configuration UI
Manager final administrative composition
```

## Testing

Automatizar comportamiento, contracts, invariants, regressions y critical flows.

No fijar CSS/markup/implementación interna accidental.

## Conflict CURRENT conocido

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No crear alias para conservar el consumer.

## Siguiente foco

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
PLANNED / NEXT / DESIGN FIRST
```

Antes de implementar, verificar serializer/provider contracts existentes y definir cómo el
store durable preserva `ProjectionRecord.dependencies`.
