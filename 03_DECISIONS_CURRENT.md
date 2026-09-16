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
| Cutover raíz limpio | CURRENT |
| No crear shims/adapters/aliases temporales para legacy | FROZEN |
| Tests no son autoridad sobre contratos SUPERSEDED | FROZEN |
| Un consumer puede quedar temporalmente roto mientras se migran contratos raíz | FROZEN |

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
Generic Atlanticus infrastructure contract where applicable
```

Usar infraestructura genérica no cambia automáticamente ownership de dominio.

Una capability ADA-specific puede consumir Source/Projection/Manager genéricos y permanecer bajo `scopes/ada`.

## Orden obligatorio de migración y validación

```text
1. fijar contrato final del dominio
2. migrar implementación del dominio
3. eliminar legacy del dominio
4. eliminar tests cuyo único propósito sea preservar legacy
5. continuar con el siguiente dominio si el consumer final todavía depende de contratos no migrados
6. cortar el consumer final una sola vez
7. ejecutar regression/qualification final
```

## Source / Projection

| Decisión | Estado |
|---|---|
| Source genérico pertenece a `web/capabilities/source` | CURRENT |
| Projection exact-release pertenece a `web/capabilities/projection/core` | CURRENT |
| Release identity != content hash | FROZEN |
| Source current nunca lo determina Cosmos | FROZEN |
| Projection target = `SourceKey + SourceReleaseRef + dependencies` | FROZEN |
| Projection dependencies son exact `ProjectionTarget` | FROZEN / IMPLEMENTED |
| dependencies se normalizan determinísticamente por `source_key` | FROZEN / IMPLEMENTED |
| `project(target)` no relee current | FROZEN |
| CURRENT/OUTDATED compara exact target | FROZEN |
| Retry conserva exact target | FROZEN |
| No introducir shim `SourceReleaseId <-> str` | FROZEN |
| Restore publica una nueva release; no repunta current | FROZEN |
| No reconstruir `ProjectionTarget` desde revision | FROZEN |
| `expected_source_revision` | SUPERSEDED / REMOVED |
| private projection revision identity | SUPERSEDED |

## Manager generic contract

| Decisión | Estado |
|---|---|
| Manager tiene un solo contrato Source/Projection genérico | FROZEN / IMPLEMENTED |
| `ManagerModule.source_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.source_reader_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.projection_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.draft_validation_service` | FROZEN / IMPLEMENTED |
| `source_history_service` opcional | FROZEN / IMPLEMENTED |
| `workflow_service` legacy | SUPERSEDED |
| campos `exact_source_*` | SUPERSEDED |
| `exact_projection_service` | SUPERSEDED |
| doble routing exact/legacy | FORBIDDEN |
| adapters/shims/aliases para conservar contrato anterior | FORBIDDEN |

## Manager Source invariants

| Decisión | Estado |
|---|---|
| Source BASE = `SourceSnapshot` | FROZEN |
| Publication recibe snapshot/concurrency semantics genéricas | FROZEN |
| conflicto se determina por release identity | FROZEN |
| cambio aislado de concurrency token no implica nueva release | FROZEN |
| History usa `HistoryPage` + `SourceReleaseRef` | FROZEN |
| History read devuelve la release solicitada | FROZEN |
| Source identity no se reduce a revision string | FROZEN |

## Manager Projection invariants

| Decisión | Estado |
|---|---|
| `ProjectionTarget` llega completo a `project(...)` | FROZEN |
| Manager no reconstruye target desde revision | FROZEN |
| `ProjectionExecutionResult.target` conserva target ejecutado | FROZEN |
| target con `source_key` distinto al módulo es inválido | FROZEN |
| no existe Manager Projection model legacy paralelo | FROZEN |

## Navigation

| Decisión | Estado |
|---|---|
| Navigation consume Manager genérico directamente | FROZEN / IMPLEMENTED |
| Navigation no tiene arquitectura Manager especial | FROZEN |
| legacy adapters/configuration stores | SUPERSEDED / REMOVED |
| `expected_source_revision` | SUPERSEDED / REMOVED |
| compatibility shims/aliases | FORBIDDEN |

## Users

| Decisión | Estado |
|---|---|
| Users Manager consume contrato genérico Manager | IMPLEMENTED / VERIFIED / CURRENT |
| `UsersProfilesConfiguration` es aggregate CURRENT | CURRENT |
| contracts/schema legacy paralelos | SUPERSEDED / REMOVED |
| adapters permanentes para historia durable | FORBIDDEN |
| migración histórica, si existe necesidad real | EXPLICIT ONE-OFF OPERATION ONLY |
| `USERS-CLEAN-CUTOVER-COMPLETION` | CLOSED / VERIFIED / CURRENT |
| `USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL` | CLOSED / VERIFIED / CURRENT |

## Tools

| Decisión | Estado |
|---|---|
| Tools ownership permanece `scopes/ada/web/tools` | FROZEN / CURRENT |
| Tool domain semantics permanecen ADA-specific | FROZEN |
| Tool Source consume `atlanticus.web.source` directamente | IMPLEMENTED / VERIFIED / CURRENT |
| Tool Projection consume `atlanticus.web.projection` directamente | IMPLEMENTED / VERIFIED / CURRENT |
| private Tool lifecycle contracts | SUPERSEDED / REMOVED |
| `expected_source_revision` in Tool domain | SUPERSEDED / REMOVED |
| private Tool projection revision | SUPERSEDED / REMOVED |
| crear adapter/alias para el consumer antiguo | FORBIDDEN |

## KPI Configuration

| Decisión | Estado |
|---|---|
| ownership permanece `scopes/ada/web/kpis/configuration` | FROZEN / CURRENT |
| dominio permanece ADA-specific | FROZEN |
| Source consume contrato genérico | IMPLEMENTED / VERIFIED / CURRENT |
| Projection consume contrato genérico | IMPLEMENTED / VERIFIED / CURRENT |
| `KpiSourceService` | CURRENT |
| `KpiProjectionBuilder` | CURRENT |
| `SourceProjectionService[KpiConfiguration]` | CURRENT |
| dependencia semántica en Tool Projection | FROZEN / IMPLEMENTED |
| dependencia exacta usa `ProjectionTarget.dependencies` | FROZEN / IMPLEMENTED |
| `KpiDestinationCatalog` conserva semántica sin revision privada | FROZEN / IMPLEMENTED |
| procedencia Tool usa `KpiDestinationCatalogSnapshot.projection_target` | FROZEN / IMPLEMENTED |
| `tool_projection_revision` como identidad privada | SUPERSEDED / REMOVED |
| private KPI Source/Projection lifecycle | SUPERSEDED / REMOVED |
| `expected_source_revision` | SUPERSEDED / REMOVED |
| revision → `ProjectionTarget` reconstruction | SUPERSEDED / REMOVED |
| tocar Manager durante este incremento | FORBIDDEN / NOT DONE |

## KPI Definition — siguiente frontera

| Decisión | Estado |
|---|---|
| ownership permanece `scopes/ada/web/kpis/definition` | FROZEN |
| cutover Source/Projection | PLANNED / NEXT |
| dependencia en KPI Configuration debe conservarse semánticamente | FROZEN |
| identidad de dependencia final usa contrato genérico Projection | FROZEN |
| revision string privada como identidad final | SUPERSEDED |
| copiar implementación KPI Configuration sin inspección | FORBIDDEN |

## Projection orchestration — refinamiento

No existe un orden global rígido de todas las proyecciones.

La afirmación anterior de que toda base projection debe ser independiente queda refinada:

```text
independent when there is no real semantic dependency
exact ProjectionTarget.dependencies when a real dependency exists
```

KPI Configuration es el caso implementado:

```text
exact Tool ProjectionTarget
    ↓
KPI Configuration ProjectionTarget.dependencies
```

Una derived resolution sigue siendo válida para materializaciones realmente derivadas, pero no sustituye una dependencia que forma parte de la identidad exacta de una projection.

## Estrategia de Configuration Manager

```text
Tools Source/Projection                    CLOSED / CURRENT
KPI Configuration Source/Projection       CLOSED / CURRENT
KPI Definition Source/Projection          PLANNED / NEXT
ADA Configuration Manager final cutover   BLOCKED
Global regression                         BLOCKED
```

No introducir parches temporales en el consumer.

## Decisiones reemplazadas o refinadas

1. Mantener la composición ejecutable durante cada migración de dominio.
   → **SUPERSEDED**.

2. Crear adapters/shims para sostener contratos legacy durante el cutover.
   → **SUPERSEDED / FORBIDDEN**.

3. Generalizar una capability ADA-specific por usar infraestructura genérica.
   → **SUPERSEDED / FORBIDDEN**.

4. Preservar private revision strings para dependencias KPI.
   → **SUPERSEDED**; la identidad exacta usa `ProjectionTarget`.

5. Tratar KPI Configuration Projection como base projection independiente de Tool Projection y resolver Tool + KPI sólo downstream.
   → **REFINED / SUPERSEDED PARA KPI CONFIGURATION**; la dependencia exacta Tool forma parte del target KPI Configuration.

## Qualification observada

| Hallazgo | Estado |
|---|---|
| checkpoint KPI Configuration publicado | VERIFIED / `4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe` |
| KPI Source/Projection code | VERIFIED / CURRENT |
| legacy KPI files removed | VERIFIED |
| Ruff scoped KPI Configuration | VERIFIED / PASS |
| pytest scoped KPI Configuration | VERIFIED / 45 passed |
| legacy token scan scoped | VERIFIED / 0 matches |
| `git diff --check` previo a publicación | VERIFIED / PASS |
| CI remoto del commit | UNVERIFIED |
| full ADA regression | BLOCKED |
| KPI Definition | PLANNED / NEXT |
| Python 3.14.7 global | UNVERIFIED |

## Conflicto abierto de Python metadata

Decisión global:

```text
Python 3.14.7
```

KPI Configuration publicado:

```text
requires-python = "==3.14.2"
```

No resolver silenciosamente dentro del cutover KPI Definition.

## Siguiente foco único

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

No tocar Manager final ni abrir limpieza transversal de Python en el mismo incremento.
