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

FORBIDDEN:

```text
mantener legacy para que el consumer siga importando
crear adapters para sostener composición temporal
reintroducir schemas/revisions anteriores como fallback
convertir una capability ADA-specific en core genérico sin necesidad real
```

## Source / Projection

| Decisión | Estado |
|---|---|
| Source genérico pertenece a `web/capabilities/source` | CURRENT |
| Projection exact-release pertenece a `web/capabilities/projection/core` | CURRENT |
| Release identity != content hash | FROZEN |
| Source current nunca lo determina Cosmos | FROZEN |
| Projection target = `SourceKey + SourceReleaseRef + dependencies` | FROZEN |
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
| `ToolSourceService` | CURRENT |
| `ToolProjectionBuilder` | CURRENT |
| `SourceProjectionService[ToolConfiguration]` | CURRENT |
| private Tool lifecycle contracts | SUPERSEDED / REMOVED |
| `ToolLifecycleServices` | SUPERSEDED / REMOVED FROM TOOLS |
| `ToolConfigurationSourceSnapshot` | SUPERSEDED / REMOVED |
| `ToolConfigurationProjectionSnapshot` | SUPERSEDED / REMOVED |
| `ToolConfigurationProjectionRepository` | SUPERSEDED / REMOVED |
| `expected_source_revision` in Tool domain | SUPERSEDED / REMOVED |
| private Tool projection revision | SUPERSEDED / REMOVED |
| mantener Configuration Manager ejecutable durante este cutover | SUPERSEDED |
| crear adapter/alias para el consumer antiguo | FORBIDDEN |

## KPI Configuration — siguiente frontera

| Decisión | Estado |
|---|---|
| ownership permanece `scopes/ada/web/kpis/configuration` | FROZEN |
| usar Tools CURRENT como referencia estructural, no copiar semántica | FROZEN |
| migrar Source a contrato genérico | PLANNED / NEXT |
| migrar Projection a contrato genérico | PLANNED / NEXT |
| conservar dependencia semántica en Tool Projection | FROZEN |
| representar identidad de dependencia mediante contrato genérico de Projection | FROZEN |
| conservar `tool_projection_revision` como identidad privada | SUPERSEDED |
| tocar Manager durante este incremento | FORBIDDEN |

## KPI Definition

| Decisión | Estado |
|---|---|
| ownership permanece `scopes/ada/web/kpis/definition` | FROZEN |
| cutover Source/Projection | PLANNED |
| dependencia en KPI Configuration debe conservarse semánticamente | FROZEN |
| revision string privada como identidad final | SUPERSEDED |

## Estrategia de Configuration Manager

```text
Tools Source/Projection                    CLOSED / CURRENT
KPI Configuration Source/Projection       PLANNED / NEXT
KPI Definition Source/Projection          PLANNED
ADA Configuration Manager final cutover   BLOCKED
Global regression                         BLOCKED
```

No introducir parches temporales en el consumer.

## Decisiones reemplazadas o refinadas

1. `TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER` como siguiente incremento directo.
   → **REFINED**: primero se migró el contrato interno Source/Projection de Tools; Manager se corta al final.

2. Mantener la composición ejecutable durante cada migración de dominio.
   → **SUPERSEDED**.

3. Crear una composición Manager dentro del paquete Tools durante su cutover.
   → **SUPERSEDED**.

4. Generalizar Tools por usar infraestructura genérica.
   → **SUPERSEDED / FORBIDDEN**.

5. Preservar private revision strings para dependencias KPI.
   → **SUPERSEDED**; usar identidad genérica de Projection.

## Qualification observada

| Hallazgo | Estado |
|---|---|
| checkpoint Tools publicado | VERIFIED / `27c2e4beed125fe379881048f0df5fbe3ff6cb1a` |
| Tool Source/Projection code | VERIFIED / CURRENT |
| tests CURRENT de Tool Source/Projection existen | VERIFIED |
| ejecución scoped posterior al cutover | UNVERIFIED |
| CI remoto del commit | UNVERIFIED / no status observado |
| full ADA regression | BLOCKED |
| KPI Configuration | PLANNED / NEXT |
| KPI Definition | PLANNED |
| Python 3.14.7 global | UNVERIFIED |

## Siguiente foco único

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

No tocar Manager final ni KPI Definition en el mismo incremento.
