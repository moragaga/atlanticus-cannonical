# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contratos CLOSED.

## CLOSED — Manager authorization semantics

```text
MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

CURRENT:

```text
ManagerModule.access_key
ManagerAuthorizationPolicy.can_view
explicit principal.access_keys
```

## CLOSED — Manager callback cardinality

```text
MANAGER-ACTIVE-WORKFLOW-CALLBACK-CARDINALITY
CLOSED / VERIFIED / CURRENT
```

La transición sin módulo resoluble usa `PreventUpdate`.

## CLOSED — configuration UI composition recovery

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
CLOSED / VERIFIED / CURRENT
```

Quedó fijado:

```text
presentation belongs to each module
reusable behavior may be generic when real reuse exists
visual similarity alone does not justify shared UI
```

El primer comportamiento transversal identificado y cerrado fue pagination.

## CLOSED — generic web pagination

```text
GENERIC-WEB-PAGINATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

CURRENT:

```text
atlanticus.web.pagination
PageRequest
Page
paginate_items
DEFAULT_PAGE_SIZE = 10
ALLOWED_PAGE_SIZES = (10, 20)
```

SUPERSEDED / REMOVED:

```text
ada.web.configuration.pagination
ConfigurationPageRequest
ConfigurationPage
DEFAULT_CONFIGURATION_PAGE_SIZE
ALLOWED_CONFIGURATION_PAGE_SIZES
```

UI/CSS/placeholders no forman parte del contrato generic.

## OPEN — Profiles Configuration editor contract

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
PLANNED / NEXT
```

Debe resolver únicamente el comportamiento del editor sobre contratos CURRENT de Profiles.

No debe:

```text
inventar nuevos modelos Profiles
crear Manager-specific lifecycle
crear UI shared framework
copiar contratos legacy Users/Profiles
publicar Source dentro del editor si no forma parte del contrato acordado
```

La Web surface queda separada.

## OPEN — Profiles Configuration Web surface

```text
PROFILES-CONFIGURATION-WEB-SURFACE
PLANNED
```

Debe tener presentación propia y puede consumir `atlanticus.web.pagination`.

Reglas UI ya decididas:

```text
full available width
page size 10 | 20
10 default
real rows remain interactive
placeholder rows, if used for stable height, are presentation-only
pagination position should remain visually stable
responsive remains presentation-specific
```

La validación visual no se reemplaza con tests de CSS/clases.

## OPEN — navigation-manager authorization consumer mismatch

Implementación CURRENT contiene:

```text
ManagerAuthorizationPolicy.can_view(...)
```

pero `web/compositions/navigation-manager` llama:

```text
resolved_authorization.can_access(...)
```

Estado:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No introducir `can_access` como alias de compatibilidad.

## OPEN — Navigation fallback para identidad no promovida

La entrada a la aplicación ya es CURRENT para identidad autenticada no promovida.

Sigue OPEN la composición exacta de `NavigationPrincipal`/perfil de fallback.

No resolver mediante:

```text
guest authority en Users
UserRecord ficticio
Navigation -> Users dependency
Navigation -> ADA Access dependency
```

## OPEN — Users Administration surface

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

El core de administración existe. Falta superficie UI.

No reintroducir Users Source/Projection.

## OPEN — ADA Access Configuration UI

ADA Access core + configuration + Source lifecycle existen.

Falta editor/surface administrativa.

```text
PLANNED / SEPARATE INCREMENT
```

No confundir esta UI con el wiring runtime exacto de ADA Access.

## OPEN — final Manager administrative composition

```text
MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED / FINAL
```

Se aborda después de cerrar las superficies faltantes. No anticipar ahora una arquitectura
especial de sidebar/navigation para cada dominio.

## OPEN — ADA Access runtime composition

```text
PLANNED / SEPARATE
```

No convertir ADA Access en dependency de Navigation.

## OPEN — concrete Entra directory discovery

Contrato disponible:

```text
UsersDirectoryReader
```

Provider concreto Graph/Entra:

```text
UNVERIFIED
```

No inventar tenant settings, Graph permissions, credential flow ni endpoints.

## OPEN — Python package metadata alignment

Canonical fija Python 3.14.7.

Packages CURRENT aún contienen metadata 3.14.2.

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## PLANNED — Web test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

Incluye deuda preexistente como import-order findings sólo cuando se tome ese foco; no
mezclar con Profiles salvo bloqueo directo.

## UNVERIFIED

```text
CI remoto de fbef06a8...
full Ruff workspace
Python/Trixie global qualification
concrete Entra/Graph provider
```
