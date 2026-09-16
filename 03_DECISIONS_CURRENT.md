# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `python:3.14.7-slim-trixie` | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `uv`, no pip normal | CURRENT |
| `backend/` representa backend jobs; no todo Python server-side | CURRENT |
| Server-side Python con responsabilidad Web pertenece a `web/` | CURRENT |
| Connectivity es dual-use y no adquiere ownership funcional | CURRENT |
| Definir contratos antes que consumidores | CURRENT |
| Cutover raíz limpio; no crear shims legacy temporales | CURRENT |

## Source / Projection

| Decisión | Estado |
|---|---|
| Source genérico pertenece a `web/capabilities/source` | CURRENT |
| Projection exact-release pertenece a `web/capabilities/projection/core` | CURRENT |
| Release identity != content hash | FROZEN |
| Source current nunca lo determina Cosmos | FROZEN |
| Projection target = `SourceKey + SourceReleaseRef` | FROZEN |
| `project(target)` no relee current | FROZEN |
| CURRENT/OUTDATED compara release identity | FROZEN |
| Retry conserva exact target | FROZEN |
| No introducir shim `SourceReleaseId <-> str` | FROZEN |
| Restore publica una nueva release; no repunta current | FROZEN |

## Manager generic contract

| Decisión | Estado |
|---|---|
| Manager tiene un solo contrato Source/Projection genérico | FROZEN / IMPLEMENTED |
| `ManagerModule` usa `source_service` | FROZEN / IMPLEMENTED |
| `ManagerModule` usa `source_reader_service` | FROZEN / IMPLEMENTED |
| `ManagerModule` usa `projection_service` | FROZEN / IMPLEMENTED |
| `ManagerModule` usa `draft_validation_service` | FROZEN / IMPLEMENTED |
| `source_history_service` es opcional | FROZEN / IMPLEMENTED |
| `workflow_service` legacy | SUPERSEDED / REMOVED FROM CURRENT CONTRACT |
| campos `exact_source_*` | SUPERSEDED / REMOVED |
| `exact_projection_service` | SUPERSEDED / REMOVED |
| `ExactSourceReaderWorkflow` | SUPERSEDED BY `SourceReaderWorkflow` |
| `ExactSourcePublicationWorkflow` | SUPERSEDED BY `SourcePublicationWorkflow` |
| `ExactSourceHistoryWorkflow` | SUPERSEDED BY `SourceHistoryWorkflow` |
| `ExactProjectionWorkflow` como frontera Manager | SUPERSEDED |
| `ConfigurationLifecycleWorkflow` como contrato activo Manager | SUPERSEDED |
| `resolve_exact_source_lifecycle` | SUPERSEDED / REMOVED |
| doble routing exact/legacy | FORBIDDEN |
| adapters/shims/aliases para conservar el contrato anterior | FORBIDDEN |

## Manager Source invariants

| Decisión | Estado |
|---|---|
| Source BASE = `SourceSnapshot` | FROZEN |
| Publication recibe `expected_source_snapshot` | FROZEN |
| conflicto se determina por release identity | FROZEN |
| cambio aislado de concurrency token no implica nueva release | FROZEN |
| Manager pasa snapshot/token fresco al workflow si la release no cambió | FROZEN |
| History usa `HistoryPage` + `SourceReleaseRef` | FROZEN |
| History read debe devolver la release solicitada | FROZEN |
| Source identity no se reduce a revision string | FROZEN |

## Manager Projection invariants

| Decisión | Estado |
|---|---|
| `ProjectionTarget` llega completo al `project(...)` | FROZEN |
| Manager no reconstruye target desde revision | FROZEN |
| `expected_source_revision` | SUPERSEDED / REMOVED |
| `ProjectionExecutionResult.target` conserva el target ejecutado | FROZEN |
| target con `source_key` distinto al módulo es inválido | FROZEN |
| no existe Manager Projection model legacy paralelo | FROZEN |

## Manager Workspace

| Decisión | Estado |
|---|---|
| `ManagerWorkspace` schema = `2` | CURRENT |
| workspace conserva `SourceSnapshot` | FROZEN |
| local revision identifica payload local | FROZEN |
| local revision != Source release identity | FROZEN |
| parser schema anterior no se adapta | FROZEN CLEAN CUTOVER |
| historical load conserva current BASE | FROZEN |
| historical load crea local dirty work | FROZEN |
| publicar después crea nueva Source release | FROZEN |

## Navigation decisions

| Decisión | Estado |
|---|---|
| Navigation consume Manager genérico directamente | FROZEN / IMPLEMENTED |
| Navigation no tiene arquitectura Manager especial | FROZEN |
| Source local usa `LocalSourceStore` | IMPLEMENTED |
| Source Azure usa `BlobSourceStore` | IMPLEMENTED |
| Projection local usa `LocalNavigationProjectionStore` | IMPLEMENTED |
| Projection Azure usa `CosmosNavigationProjectionStore` | IMPLEMENTED |
| legacy adapters/configuration stores paralelos | SUPERSEDED / REMOVED |
| `expected_source_revision` en Navigation | SUPERSEDED / REMOVED |
| browser source revision ejecutable | SUPERSEDED / REMOVED |
| compatibility shims/aliases | FORBIDDEN |

## Decisions superseded/refined

1. Navigation estaba `PLANNED / NEXT`.
   → **SUPERSEDED**: `NAVIGATION-GENERIC-CONFIGURATION-CUTOVER` está `CLOSED / VERIFIED / CURRENT`.

2. La migración de Navigation podía conservar adapters legacy durante transición.
   → **SUPERSEDED / FORBIDDEN**: el cierre fue limpio, sin convivencia.

3. La qualification global podía esperarse inmediatamente después de Navigation.
   → **REFINED**: la suite global reveló una desalineación preexistente en `users-manager`; no se adjudica ni corrige dentro del incremento de Navigation.

4. El siguiente paso podía asumirse como implementación de Users.
   → **REFINED**: el siguiente paso es sólo `USERS-MANAGER-ALIGNMENT-VALIDATION`.

5. Un fallo global posterior a Navigation implica regresión de Navigation.
   → **SUPERSEDED AS ASSUMPTION**: la causalidad debe verificarse por checkpoint y archivos cambiados.

## Qualification

| Hallazgo | Estado |
|---|---|
| Current implementation checkpoint | VERIFIED / `d34cda3838a67907728b382e238f0178f9f1a64e` |
| Parent | VERIFIED / `59fcd3ecc8f3441e64fbe0fc892b4467fa56f181` |
| Navigation scoped Ruff | VERIFIED / PASS |
| Navigation + Manager scoped tests | VERIFIED / `102 passed` |
| Navigation forbidden legacy scan | VERIFIED / `0 results` |
| `git diff --check` | VERIFIED / PASS |
| `git diff --cached --check` | VERIFIED / PASS |
| Full Web suite | BLOCKED DURING COLLECTION |
| Users Manager alignment | VERIFIED MISALIGNMENT / SOLUTION UNVERIFIED |
| Tools consumer | UNVERIFIED |
| KPI Configuration consumer | UNVERIFIED |
| KPI Definition consumer | UNVERIFIED |
| Docker E2E | UNVERIFIED |
| Python 3.14.7 global qualification | UNVERIFIED |

## Status de hitos

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER          CLOSED / VERIFIED / CURRENT
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER          CLOSED / VERIFIED / CURRENT

USERS-MANAGER-ALIGNMENT-VALIDATION                 PLANNED / NEXT

TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER             PLANNED
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER       PLANNED
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER   PLANNED

MANAGER-CONSUMER-GLOBAL-QUALIFICATION              BLOCKED
```

## Siguiente decisión de ejecución

Único foco recomendado:

```text
USERS-MANAGER-ALIGNMENT-VALIDATION
```

Usar obligatoriamente `atlanticus:main` y `atlanticus-cannonical:main`.

No decidir implementación antes de validar la desalineación.
