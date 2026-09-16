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
| `ManagerModule.source_key` | FROZEN / IMPLEMENTED |
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

## KPI Definition

| Decisión | Estado |
|---|---|
| ownership permanece `scopes/ada/web/kpis/definition` | FROZEN / CURRENT |
| dominio permanece ADA-specific | FROZEN |
| Source consume contrato genérico | IMPLEMENTED / VERIFIED / CURRENT |
| Projection consume contrato genérico | IMPLEMENTED / VERIFIED / CURRENT |
| `KpiDefinitionSourceService` | CURRENT |
| `KpiDefinitionProjectionBuilder` | CURRENT |
| `SourceProjectionService[KpiDefinitionCatalog]` | CURRENT |
| dependencia semántica en KPI Configuration Projection | FROZEN / IMPLEMENTED |
| dependencia exacta usa `ProjectionTarget.dependencies` | FROZEN / IMPLEMENTED |
| exactamente una dependencia KPI Configuration | FROZEN / IMPLEMENTED |
| Definition ausente para KPI configurado se materializa `MISSING` | FROZEN / IMPLEMENTED |
| Definition huérfana para KPI no configurado invalida la proyección | FROZEN / IMPLEMENTED |
| `KpiDefinitionAuthorityCatalog` / `KpiDefinitionAuthorityProvider` | SUPERSEDED / REMOVED |
| `KpiDefinitionServices` / private lifecycle | SUPERSEDED / REMOVED |
| `kpi_configuration_revision` como identidad privada | SUPERSEDED / REMOVED |
| `expected_source_revision` | SUPERSEDED / REMOVED |
| `build_kpi_definition_digest` como identidad Source/workspace | SUPERSEDED / REMOVED |
| revision → `ProjectionTarget` reconstruction | SUPERSEDED / REMOVED |
| adapters/aliases para sostener `ada-configuration-manager` antiguo | FORBIDDEN |

## Projection orchestration — refinamiento

No existe un orden global rígido de todas las proyecciones.

Regla CURRENT:

```text
independent when there is no real semantic dependency
exact ProjectionTarget.dependencies when a real dependency exists
```

Cadena implementada:

```text
Tool ProjectionTarget
    ↓ exact dependency
KPI Configuration ProjectionTarget
    ↓ exact dependency
KPI Definition ProjectionTarget
```

Una derived resolution sigue siendo válida para materializaciones realmente derivadas, pero no sustituye una dependencia que forma parte de la identidad exacta de una projection.

## Estrategia de Configuration Manager

```text
Users Manager contract                     CLOSED / CURRENT
Navigation Manager contract                CLOSED / CURRENT
Tools Source/Projection                    CLOSED / CURRENT
KPI Configuration Source/Projection        CLOSED / CURRENT
KPI Definition Source/Projection           CLOSED / CURRENT
ADA Configuration Manager final cutover    PLANNED / NEXT
Global regression                          BLOCKED
Web test contract cleanup                  PLANNED / AFTER MANAGER
```

Todos los contratos de dominio requeridos para el consumer final ya están disponibles.

No introducir parches temporales en `ada-configuration-manager`.

El siguiente incremento debe cortar el consumer completo al contrato Manager genérico CURRENT.

## Decisiones reemplazadas o refinadas

1. Mantener la composición ejecutable durante cada migración de dominio.
   → **SUPERSEDED**.

2. Crear adapters/shims para sostener contratos legacy durante el cutover.
   → **SUPERSEDED / FORBIDDEN**.

3. Generalizar una capability ADA-specific por usar infraestructura genérica.
   → **SUPERSEDED / FORBIDDEN**.

4. Preservar private revision strings para dependencias KPI.
   → **SUPERSEDED**; la identidad exacta usa `ProjectionTarget`.

5. Tratar KPI Configuration Projection como independiente de Tool Projection.
   → **REFINED / SUPERSEDED PARA KPI CONFIGURATION**; la dependencia Tool forma parte del target exacto.

6. Modelar KPI Definition mediante un `KpiDefinitionAuthority` intermedio basado en revision/keys.
   → **SUPERSEDED / REMOVED**; KPI Definition consume directamente la proyección tipada de KPI Configuration.

7. Hacer un cutover específico sólo del consumer KPI Definition dentro de `ada-configuration-manager`.
   → **SUPERSEDED / REFINED**; como todos los dominios Configuration ya están migrados, el próximo corte es el Configuration Manager completo.

8. Mantener `ADA-CONFIGURATION-MANAGER-FINAL-CUTOVER` bloqueado por KPI Definition.
   → **SUPERSEDED**; KPI Definition está CLOSED y el consumer final queda `PLANNED / NEXT`.

## Qualification observada

| Hallazgo | Estado |
|---|---|
| checkpoint KPI Definition publicado | VERIFIED / `ef3f0a44c5dcc14f8fcafe5bb36bb97865381924` |
| KPI Definition Source/Projection code | VERIFIED / CURRENT |
| legacy KPI Definition API removida del package CURRENT | VERIFIED |
| Ruff scoped KPI Definition | VERIFIED / PASS |
| pytest scoped KPI Definition | VERIFIED / 40 passed |
| legacy token scan scoped | VERIFIED / 0 matches |
| CI remoto del commit | UNVERIFIED |
| full ADA regression | BLOCKED |
| Configuration Manager final cutover | PLANNED / NEXT |
| Python 3.14.7 global | UNVERIFIED |

## Conflicto abierto de Python metadata

Decisión global:

```text
Python 3.14.7
```

Implementación publicada:

```text
KPI Configuration requires-python = "==3.14.2"
KPI Definition    requires-python = "==3.14.2"
```

No resolver silenciosamente dentro del Configuration Manager final.

## Siguiente foco único

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

No reabrir dominios ya cerrados ni mezclar limpieza transversal de Python o tests Web en el mismo incremento.
