# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada y verificada:

```text
moragaga/atlanticus@a065f45c55a527c96ce333705465487e95f0a737
```

Parent inmediato:

```text
ec9bd35455b8221180b3f15740b58e34766f6112
```

La qualification final terminó con working tree limpio.

## Estado resumido

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER          CLOSED / VERIFIED / CURRENT
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER           CLOSED / VERIFIED / CURRENT
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER             CLOSED / VERIFIED / CURRENT
USERS-CLEAN-CUTOVER-COMPLETION                     CLOSED / VERIFIED / CURRENT
USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL        CLOSED / VERIFIED / CURRENT
PROJECTION-CORE-STALE-TEST-ALIGNMENT               CLOSED / VERIFIED

TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER             PLANNED / NEXT
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER        PLANNED
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER    PLANNED
```

## VERIFIED

### Manager

Contrato vigente:

```text
ManagerModule
├── source_key
├── source_service
├── source_reader_service
├── projection_service
├── draft_validation_service
└── source_history_service | None
```

No forman parte de la frontera CURRENT:

```text
workflow_service
ExactSource*
ExactProjection* como frontera Manager
expected_source_revision
revision -> ProjectionTarget reconstruction
```

### Users

`web/compositions/users-manager` consume Manager genérico.

El checkpoint `a065f45c...` elimina la compatibilidad schema v1 que bloqueaba el cierre, incluyendo ambos `schema_v1.py` y los fallbacks Source/Projection.

Familia CURRENT:

```text
UsersConfiguration
ProfilesConfiguration
UsersProfilesConfiguration
UsersSourceService
SourceSnapshot
SourceReleaseRef
ProjectionTarget
ProjectionRecord
ProjectionStore
SourceProjectionService
```

No se reintrodujo adapter, shim, alias ni segunda ruta runtime.

### Qualification final

```text
ruff scoped
PASS

pytest scoped
99 passed

forbidden scan sobre código CURRENT
PASS / zero matches

full Web pytest
545 passed
7 skipped
0 failed

git diff --check HEAD^..HEAD
PASS

git status --short
CLEAN
```

## INFERRED

La qualification prueba coherencia del Web workspace con los tests CURRENT observados y con el clean cutover inspeccionado.

No prueba automáticamente ADA, Docker E2E, CI remoto, Python/Trixie global ni consumers no inspeccionados.

## ASSUMED

No se asume existencia de datos productivos schema v1.

No se diseñó migración histórica. Si aparece una necesidad real, debe verificarse desde datos/entorno autoritativo y resolverse como operación explícita separada.

## PROPOSED

Único foco siguiente:

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
```

Primera etapa: inspección y diseño. No escribir código antes de verificar una desviación real.

## SUPERSEDED

```text
keep schema-v1 read compatibility for durable history
SUPERSEDED / REMOVED

Users is CLOSED because tests are green
SUPERSEDED

preserve old schemas to keep tests passing
SUPERSEDED
```

El cierre actual se basa en clean cutover publicado más qualification posterior.

## UNVERIFIED

- full ADA suite;
- Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global;
- Tools consumer;
- KPI Configuration consumer;
- KPI Definition consumer;
- existencia de datos históricos schema v1 que requieran migración operacional.

## Siguiente frontera

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED / NEXT
```

No mezclar KPI, Python migration, Docker E2E general, ADA-specific work ni rediseño de Manager core.
