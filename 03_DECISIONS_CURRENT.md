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

## Decisions superseded/refined

1. `ManagerModule` podía declarar un lifecycle legacy o capabilities exactas separadas.
   → **SUPERSEDED**: existe un único contrato genérico.

2. `workflow_service=None` distinguía un módulo exacto.
   → **SUPERSEDED**: `workflow_service` ya no forma parte del contrato vigente.

3. `ExactSource*Workflow` era la frontera final.
   → **SUPERSEDED**: los contratos finales son `Source*Workflow` genéricos.

4. `ExactProjectionWorkflow` era la frontera final.
   → **SUPERSEDED**: Manager consume directamente el contrato genérico de Projection.

5. podían coexistir exact y legacy durante transición.
   → **SUPERSEDED / FORBIDDEN**: no hay transición ni doble contrato.

6. revision textual podía participar en selección/ejecución de Projection.
   → **SUPERSEDED / FORBIDDEN**: `ProjectionTarget` completo es el único target ejecutable.

7. `expected_source_revision`.
   → **SUPERSEDED / REMOVED**: publication usa `SourceSnapshot`.

8. la migración de los cuatro consumidores podía abordarse como un único incremento.
   → **REFINED**: un chat/incremento por consumidor para conservar trazabilidad.

## Qualification

| Hallazgo | Estado |
|---|---|
| Current implementation checkpoint | VERIFIED / `59fcd3ecc8f3441e64fbe0fc892b4467fa56f181` |
| Parent | VERIFIED / `1302fefdf046b1cef7beed594e832f9a7a181a06` |
| Manager capability suite | VERIFIED / `54 passed` |
| Full Web suite en current checkpoint | UNVERIFIED |
| Full ADA suite en current checkpoint | UNVERIFIED |
| Navigation consumer contra contrato nuevo | UNVERIFIED |
| Tools consumer contra contrato nuevo | UNVERIFIED |
| KPI Configuration consumer contra contrato nuevo | UNVERIFIED |
| KPI Definition consumer contra contrato nuevo | UNVERIFIED |
| Docker E2E | UNVERIFIED |
| Python 3.14.7 qualification current checkpoint | UNVERIFIED |

Los conteos anteriores `238 passed` y `56 passed / 4 failed` permanecen como evidencia histórica de otro checkpoint, no como qualification de `59fcd3e...`.

## Status de hitos

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER       CLOSED / VERIFIED / CURRENT

NAVIGATION-MANAGER-GENERIC-CONSUMER-CUTOVER     PLANNED / NEXT
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER          PLANNED
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER     PLANNED
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER PLANNED

MANAGER-CONSUMER-GLOBAL-QUALIFICATION            BLOCKED
```

## Siguiente decisión de ejecución

Único foco recomendado:

```text
NAVIGATION-MANAGER-GENERIC-CONSUMER-CUTOVER
```

No mezclar Tools, KPI Configuration ni KPI Definition.
