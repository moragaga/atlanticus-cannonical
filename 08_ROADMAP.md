# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Un solo foco por incremento.

Cerrar cada frontera con evidencia suficiente.

No conservar legacy para sostener consumers o tests anteriores.

No mezclar cleanup transversal con el incremento funcional activo.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@df5b99502265758e873e0565abf2176cc617104b
```

Parent:

```text
31723a108ddd2f49346fdcbb844db9891eb08f4b
```

## Hitos cerrados relevantes

```text
GENERIC-WEB-PAGINATION-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-EDITOR-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ACCESS-UNRESTRICTED-PROFILES-CONTRACT
CLOSED / VERIFIED / CURRENT

ACCESS-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT

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
```

## Hitos superados

```text
Navigation Configuration -> Profiles core hard dependency
SUPERSEDED / REMOVED

Navigation empty allowed_profiles denies ordinary principals
SUPERSEDED

Navigation separate profiles card
SUPERSEDED / REMOVED

Navigation guest auto-selection
SUPERSEDED / REMOVED

Navigation boundary test based on raw "ada." substring
SUPERSEDED / REMOVED

Access root/local explicit grants
SUPERSEDED / FORBIDDEN

Access inline profile multiselect
SUPERSEDED / REMOVED

Access profile assignment overlay store
SUPERSEDED / REMOVED

Profiles dbc.Modal editor dependency
SUPERSEDED / REMOVED

Profiles previous/next-only pagination presentation
SUPERSEDED
```

## Finding no cerrado

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No resolver con alias/shim.

## Foco activo

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Slices cerrados:

```text
Navigation
Accesos
Perfiles
```

Siguiente página acordada:

```text
Users
```

`Herramienta` continúa `OPEN / DEFERRED`; el cambio de orden no la supersede ni la cierra.

## Secuencia interna del foco activo

```text
PHASE 1 — PAGE VISUAL REVIEW
CURRENT / IN PROGRESS
```

Siguiente slice: Users.

```text
PHASE 2 — RESPONSIVE / MEDIA QUERY AUDIT
PLANNED
```

No abrir transversalmente durante el slice Users salvo defecto compartido demostrado.

```text
PHASE 3 — TEST CONTRACT QUALIFICATION
PLANNED
```

Ejecutar targeted suites y Ruff cuando corresponda al cierre del alcance actual.

Eliminar tests que sólo congelen CSS, markup, clases/funciones internas o detalles visuales.

## Punto de cierre después de Users

El usuario indicó que, una vez cerrado Users, desea cerrar el trabajo de Manager **por ahora**.

Ese punto de cierre debe registrar explícitamente qué frentes quedan diferidos. En particular:

```text
Herramienta visual review
OPEN / DEFERRED
```

No convertir ese diferimiento en un cierre ficticio.

## Después del UI review

```text
MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW
```

No abrirlo durante Users.

## Frentes separados

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT

ADA Access runtime composition
PLANNED / SEPARATE

Navigation disabled-route surface
PLANNED / SEPARATE

concrete Entra/Graph provider
UNVERIFIED

PYTHON-METADATA-ALIGNMENT
PLANNED / SEPARATE

CI remote
UNVERIFIED

full workspace qualification
UNVERIFIED
```
