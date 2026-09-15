# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades.

No reinterpretar un FAIL histórico como fallo vigente sin revisar su checkpoint y adjudicación.

No declarar GREEN global cuando sólo existe qualification scoped.

## Checkpoint actual de este cierre

```text
moragaga/atlanticus@59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
parent: 1302fefdf046b1cef7beed594e832f9a7a181a06
```

## Manager generic Source/Projection cutover

Estado:

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Propiedades verificadas por implementación inspeccionada:

- `ManagerModule` expone una sola familia genérica de servicios;
- no contiene `workflow_service`;
- no contiene campos `exact_source_*`;
- no contiene `exact_projection_service`;
- Manager Source usa `SourceSnapshot`, `SourceReleaseRef`, `HistoryPage` y `PublishResult`;
- Manager Projection consume `ProjectionStatus`, `ProjectionTarget` y `ProjectionExecutionResult` de `projection/core`;
- `project(...)` recibe `ProjectionTarget`;
- no hay reconstrucción revision→target;
- publication usa `expected_source_snapshot`;
- conflicto se determina por release identity;
- token de concurrencia fresco se conserva cuando la release no cambió;
- History read debe devolver la release solicitada;
- workspace schema vigente es `2`;
- la ruta exact/legacy anterior fue removida del Manager;
- archivos `exact_*` del Manager fueron eliminados.

## Qualification observada

Ejecutada por el usuario en el workspace real después de aplicar el cutover:

```text
web/capabilities/manager
54 passed
0 failed
```

Esta suite es la evidencia funcional vigente del cierre.

## Evidencia histórica que NO sustituye current

Los siguientes conteos pertenecen a checkpoints anteriores:

```text
238 passed
56 passed / 4 failed
```

Pueden conservarse en ledger histórico, pero no describen `59fcd3e...`.

## Lo que current checkpoint NO demuestra

UNVERIFIED:

- full Web suite;
- full ADA suite;
- consumidores Navigation/Tools/KPI Configuration/KPI Definition ya alineados;
- suites de esos consumidores contra la nueva API;
- Docker E2E;
- CI remoto adicional;
- qualification global Python 3.14.7/Trixie;
- ausencia total de residuos legacy fuera de `web/capabilities/manager`.

## Regla para consumidores

Cada consumer cutover debe producir su propia evidence set:

```text
consumer implementation inspected
consumer tests GREEN
relevant composition tests GREEN
forbidden legacy scan scoped
no adapters/shims/aliases
```

No cerrar el consumer siguiente por inferencia desde los `54 passed` de Manager.

## Git

Git continúa READ ONLY para el asistente salvo autorización explícita.
