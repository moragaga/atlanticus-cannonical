# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.

Git permanece READ ONLY salvo autorización explícita.

## Checkpoint actual

```text
moragaga/atlanticus@59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
parent: 1302fefdf046b1cef7beed594e832f9a7a181a06
```

GitHub confirma ese commit y parent.

Canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical@d4681dc3d14b0233c857ca3870795368456c7bad
```

## Cambio implementado

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

El commit reemplaza el diseño de coexistencia exact/legacy por un contrato único.

## Archivos legacy removidos de Manager

Productivo:

```text
web/capabilities/manager/src/atlanticus/web/manager/exact_projection.py
web/capabilities/manager/src/atlanticus/web/manager/exact_source.py
web/capabilities/manager/src/atlanticus/web/manager/exact_workspace.py
web/capabilities/manager/src/atlanticus/web/manager/web/exact_workspace.py
```

Espejo comentado equivalente:

```text
web/capabilities/manager/commented/atlanticus/web/manager/exact_projection.py
web/capabilities/manager/commented/atlanticus/web/manager/exact_source.py
web/capabilities/manager/commented/atlanticus/web/manager/exact_workspace.py
web/capabilities/manager/commented/atlanticus/web/manager/web/exact_workspace.py
```

También fueron retirados tests cuyo contrato era la arquitectura anterior o estructura visual no contractual.

## Contrato inspeccionado en main

`ManagerModule` CURRENT:

```text
source_key
source_service
source_reader_service
projection_service
draft_validation_service
source_history_service | None
```

Source CURRENT:

```text
SourceReaderWorkflow
SourcePublicationWorkflow
SourceHistoryWorkflow
```

Projection CURRENT:

```text
ProjectionStatus
ProjectionTarget
ProjectionExecutionResult
```

Workspace CURRENT:

```text
ManagerWorkspace schema 2
BASE = SourceSnapshot
```

## Qualification observada

Reportada por el usuario después de aplicar el cutover:

```text
web/capabilities/manager
54 passed
0 failed
```

## Evidence scope

VERIFIED:

- commit `59fcd3e...` existe en `atlanticus:main`;
- parent `1302fefd...`;
- Manager contract genérico;
- eliminación de la ruta exact/legacy dentro de Manager;
- Manager scoped tests GREEN.

UNVERIFIED:

- full Web suite;
- full ADA suite;
- Navigation consumer;
- Tools consumer;
- KPI Configuration consumer;
- KPI Definition consumer;
- Docker E2E;
- Python 3.14.7 global;
- CI remoto adicional.

## Canonical conflict encontrado

El canonical anterior todavía describía:

- `workflow_service`;
- `exact_source_*`;
- `exact_projection_service`;
- `ExactSource*Workflow`;
- `ExactProjectionWorkflow`;
- coexistencia exact/legacy;
- `ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT` como un único frente conjunto.

Eso contradice el contrato implementado en `59fcd3e...`.

Este reemplazo adjudica el conflicto a favor de `atlanticus:main`.

## Decisions histórico

Se inspeccionó `ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md`.

Sus reglas de aplicación — Manager genérico, Home real, registry como fuente única, separación configuration/workflow — son compatibles con el cutover.

El documento usa “revisión” como concepto de UX/workflow, pero no define un contrato Source/Projection que obligue a conservar revision strings como identidad ejecutable.

No se realizó auditoría exhaustiva de todos los archivos históricos de `atlanticus-decisions`; cualquier otro conflicto permanece UNVERIFIED y no bloquea este cierre.

## Próximo ledger frontier

```text
NAVIGATION-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED / NEXT
```

Después, en chats separados:

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER
```

No reabrir Manager core.
