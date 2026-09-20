# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

Un solo foco por incremento.

Cerrar cada frontera con evidencia suficiente.

No conservar legacy para sostener consumers o tests anteriores.

No mezclar cleanup transversal con el incremento funcional activo.

## Checkpoint publicado de referencia

```text
moragaga/atlanticus@29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
```

Parent:

```text
856498c52f182cd531deae845c25bd51ae2ff4ea
```

## Hitos cerrados relevantes

```text
GENERIC-WEB-PAGINATION-CUTOVER
CLOSED / VERIFIED / CURRENT

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

Navigation ya cerró su slice visual.

Siguiente página:

```text
Herramienta
```

## Secuencia interna del foco activo

```text
PHASE 1 — PAGE VISUAL REVIEW
CURRENT / IN PROGRESS
```

Revisar las páginas Manager una por una hasta que desktop/presentación principal sea coherente.

```text
PHASE 2 — RESPONSIVE / MEDIA QUERY AUDIT
PLANNED
```

Revisar shared Manager CSS y CSS capability-local.

No conservar cambios arbitrarios por inercia.

No revertir a ciegas: comparar comportamiento visual esperado y ownership real.

```text
PHASE 3 — TEST CONTRACT QUALIFICATION
PLANNED
```

Ejecutar targeted suites y Ruff.

Eliminar tests que sólo congelen CSS, markup, clases/funciones internas o detalles visuales.

Conservar/reemplazar únicamente cuando exista comportamiento contractual real detrás.

## Después del UI review

```text
MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW
```

Debe comprobar:

```text
edit
save draft
validate
publish Source
project
reload
persistence
conflicts/retry where applicable
```

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
