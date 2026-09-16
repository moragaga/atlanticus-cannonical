# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `python:3.14.7-slim-trixie` | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `uv`, no pip normal | CURRENT |
| Definir contratos antes que consumidores | CURRENT |
| Cutover raíz limpio | CURRENT |
| No crear shims/adapters/aliases temporales para legacy | FROZEN |
| Tests no son autoridad sobre contratos SUPERSEDED | FROZEN |
| Un consumer puede quedar temporalmente roto durante un root cutover | FROZEN |

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

## Source / Projection

| Decisión | Estado |
|---|---|
| Source genérico pertenece a `web/capabilities/source` | CURRENT |
| Projection exact-release pertenece a `web/capabilities/projection/core` | CURRENT |
| Release identity != content hash | FROZEN |
| Source current nunca lo determina Cosmos | FROZEN |
| Projection target = `SourceKey + SourceReleaseRef + dependencies` | FROZEN |
| Projection dependencies son exact `ProjectionTarget` | FROZEN / IMPLEMENTED |
| `project(target)` no relee current | FROZEN |
| CURRENT/OUTDATED compara exact target | FROZEN |
| No reconstruir `ProjectionTarget` desde revision | FROZEN |
| `expected_source_revision` | SUPERSEDED / REMOVED |
| private projection revision identity | SUPERSEDED / REMOVED |

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
| `workflow_service` legacy | SUPERSEDED / REMOVED |
| campos `exact_source_*` | SUPERSEDED / REMOVED |
| `exact_projection_service` | SUPERSEDED / REMOVED |
| doble routing exact/legacy | FORBIDDEN |
| adapters/shims/aliases para conservar contrato anterior | FORBIDDEN |

## Manager Source invariants

| Decisión | Estado |
|---|---|
| Source BASE = `SourceSnapshot` | FROZEN |
| Publication recibe snapshot/concurrency semantics genéricas | FROZEN |
| conflicto se determina por release identity | FROZEN |
| History usa `HistoryPage` + `SourceReleaseRef` | FROZEN |
| Source identity no se reduce a revision string | FROZEN |

## Manager Projection invariants

| Decisión | Estado |
|---|---|
| `ProjectionTarget` llega completo a `project(...)` | FROZEN |
| Manager no reconstruye target desde revision | FROZEN |
| target con `source_key` distinto al módulo es inválido | FROZEN |
| no existe Manager Projection model legacy paralelo | FROZEN |

## Manager Workspace invariants

| Decisión | Estado |
|---|---|
| Workspace es identidad editable local, no Source identity | FROZEN |
| `ManagerWorkspace` conserva BASE como `SourceSnapshot` | FROZEN |
| `build_workspace_revision(payload)` sólo identifica payload local | FROZEN |
| un editor puede usar un bridge fino hacia `ManagerWorkspace` | IMPLEMENTED / CURRENT |
| bridge de workspace no puede reconstruir Source desde revision | FROZEN |

## Configuration domains

```text
Navigation
CLOSED / VERIFIED / CURRENT

Users
CLOSED / VERIFIED / CURRENT

Tools Source/Projection
CLOSED / VERIFIED / CURRENT

KPI Configuration Source/Projection
CLOSED / VERIFIED / CURRENT

KPI Definition Source/Projection
CLOSED / VERIFIED / CURRENT
```

Los ownerships ADA-specific permanecen bajo `scopes/ada` donde corresponda.

## ADA Configuration Manager

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT
```

La composición final publicada consume los contratos genéricos CURRENT.

La composición registra por módulo servicios separados:

```text
source
source-reader
source-history
projection
draft-validation
```

Los workflows de composición que permanecen son implementaciones directas de contratos genéricos reales. No son adapters para conservar un lifecycle anterior.

`KpiDefinitionAuthority` no forma parte del consumer final.

`ToolLifecycleServices`, `KpiConfigurationServices`, `KpiDefinitionServices`, `NavigationConfigurationServices`, `ExactProjectionWorkflow`, `expected_source_revision` y revision-string projection routing no forman parte del consumer final.

## Local runtime

Existe una composición local explícita para smoke/manual validation:

```text
LocalSourceStore
+
InProcessProjectionStore
```

Esta composición es una frontera local de ejecución del Configuration Manager.

No define por sí sola la topología productiva ni reemplaza el E2E posterior con Storage/Cosmos.

## Testing

La política permanece:

```text
test behavior/contracts/invariants
do not freeze CSS visual structure
do not preserve legacy implementation details
```

La validación visual es legítima para UI.

E2E se hará en incrementos posteriores y separados.

## Decisiones reemplazadas o refinadas

1. `ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER` como trabajo siguiente.
   → **SUPERSEDED / CLOSED**.

2. `MANAGER-CONSUMER-GLOBAL-QUALIFICATION` bloqueado por el cutover.
   → **REFINED / UNBLOCKED**, todavía no ejecutado completamente.

3. La afirmación de que el consumer CURRENT usa contratos legacy.
   → **SUPERSEDED** por `main@ee9a0401...`.

4. Resolver UI, E2E local y E2E con infraestructura dentro del mismo incremento.
   → **NOT ADOPTED**. Se mantienen como fronteras separadas.

5. Tratar cualquier “contrato raro” observado en UI como motivo automático de rediseño.
   → **FORBIDDEN**. Primero reproducir y contrastar contra contratos CURRENT.

## Qualification observada para este checkpoint

| Hallazgo | Estado |
|---|---|
| checkpoint publicado | VERIFIED / `ee9a0401c7947f2bf61abc0a783dfa905443b6b1` |
| consumer generic wiring presente | VERIFIED / CURRENT |
| `ManagerWorkspaceBridge` presente | VERIFIED / CURRENT |
| runtime local presente | VERIFIED / CURRENT |
| `git diff --check` antes de publicación | VERIFIED / PASS |
| legacy token scan scoped | VERIFIED / 0 matches |
| `compileall` scoped | VERIFIED / PASS |
| página Manager local levanta | VERIFIED / manual smoke |
| full Ruff del checkpoint | UNVERIFIED |
| full pytest del checkpoint | UNVERIFIED |
| full ADA regression | UNVERIFIED |
| local behavioral E2E | UNVERIFIED |
| Storage/Cosmos Docker E2E | UNVERIFIED |
| CI remoto | UNVERIFIED |

## Conflicto abierto de Python metadata

Decisión global:

```text
Python 3.14.7
```

Configuration Manager CURRENT:

```text
requires-python = "==3.14.2"
```

No resolver silenciosamente durante UI cleanup.

## Siguiente foco único

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
PLANNED / NEXT
```

No mezclar E2E local, Storage/Cosmos Docker, baseline Python ni otros dominios en ese incremento.
