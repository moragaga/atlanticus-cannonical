# Web Platform — Open Items

Estado: **PLANNED OPEN ITEMS**

Los items cerrados no deben reabrirse para restaurar simetría o legacy.

## Closed baselines relevantes

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-PERSISTED-DATA-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
CLOSED / VERIFIED / CURRENT

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILES-CONFIGURATION
CLOSED / VERIFIED / CURRENT

NONPROMOTED-ACCESS-SEMANTICS-CORRECTION
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT

MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT
CLOSED / VERIFIED / CURRENT

MANAGER-ACTIVE-WORKFLOW-CALLBACK-CARDINALITY
CLOSED / VERIFIED / CURRENT

CONFIGURATION-UI-COMPOSITION-RECOVERY
CLOSED / VERIFIED / CURRENT

GENERIC-WEB-PAGINATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Profiles Configuration editor contract — NEXT

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
PLANNED / NEXT
```

Consumir contratos existentes:

```text
ProfileDefinition
ProfileCatalog
ProfilesConfiguration
Profiles Source lifecycle
atlanticus.web.pagination
```

No inventar dominio nuevo ni shared UI.

El editor contract debe cerrarse antes de la Web surface.

## Profiles Configuration Web surface

```text
PROFILES-CONFIGURATION-WEB-SURFACE
PLANNED
```

La presentación es Profiles-owned.

Puede usar paginación generic pero no importar presentación ADA.

## navigation-manager authorization consumer

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`can_access` debe alinearse al contrato CURRENT `can_view` cuando entre al scope.
No crear compatibility alias.

## Users Administration

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

Usar `UsersAdministrationService` y contracts actuales.
No reintroducir Users Source/Projection.

## ADA Access Configuration UI

```text
PLANNED / SEPARATE INCREMENT
```

Usar `AdaAccessConfiguration` y contracts actuales.
Mantener ownership ADA.

## Manager final administrative composition

```text
MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED / FINAL
```

Sólo después de cerrar las superficies faltantes.

## Navigation runtime fallback

```text
PLANNED / SEPARATE
```

No crear Users authority `guest` ni UserRecord ficticio.

## ADA Access runtime

```text
PLANNED / SEPARATE
```

No convertirlo en dependency de Navigation.

## Entra / Directory

Provider concreto:

```text
UNVERIFIED
```

No inventar Graph settings/scopes/endpoints.

## Test contract cleanup

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

Incluye deuda preexistente fuera de los incrementos funcionales cuando corresponda.

## Python baseline

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## Qualification transversal

```text
CI remote fbef06a8...
UNVERIFIED

full Ruff workspace
UNVERIFIED
```
