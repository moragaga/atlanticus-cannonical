# Web Platform — Open Items

Estado: **PLANNED OPEN ITEMS**

Los items cerrados no deben reabrirse para restaurar simetría o legacy.

## Closed baselines relevantes

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
CLOSED / VERIFIED / CURRENT

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

NAVIGATION-CONFIGURATION-UI-PASS
CLOSED / VERIFIED MANUAL / CURRENT
```

## Manager UI consistency — CURRENT

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Siguiente página:

```text
Herramienta
```

No abrir backend contracts nuevos durante este review.

## Responsive/media queries — PHASE 2

```text
MANAGER-RESPONSIVE-MEDIA-QUERY-AUDIT
PLANNED
```

Debe revisar shared Manager CSS y estilos de cada capability.

Existe un cambio CURRENT de bottom padding en `atlanticus-manager__module-page` que no está
calificado para todos los breakpoints.

## Test cleanup — PHASE 3

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED
```

Eliminar tests de CSS/visual structure/internal classes/functions.

Conservar behavior contracts.

Ejecutar targeted pytest/Ruff al final del UI review.

## Navigation Manager authorization consumer

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No crear compatibility alias.

## ADA Access runtime

```text
PLANNED / SEPARATE
```

## Navigation runtime/disabled-route work

```text
PLANNED / SEPARATE
```

No mezclar con UI review.

## Entra / Directory

```text
concrete provider
UNVERIFIED
```

## Python baseline

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / SEPARATE
```

## Qualification transversal

```text
post-29bb targeted suite
UNVERIFIED

CI remote
UNVERIFIED

full Ruff workspace
UNVERIFIED
```
