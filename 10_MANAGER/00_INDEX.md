# Manager — Canonical Index

Estado: **CURRENT / GENERIC SOURCE-PROJECTION CUTOVER CLOSED**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como aplicación/capability independiente y ownership de shell. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home `/manager`, sidebar, registry y navegación administrativa. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION y contrato genérico único. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Herramienta, Component/Subcomponent, KPI y alarmas. | FROZEN/CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection exact-release consumido por Manager genérico. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual vs qualification visual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuentes/checkpoints/evidencia del Manager actual. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Primera instalación, bypass y acceso pre-Manager. | CURRENT DIRECTION |
| `09_ADA_COMPONENT_LINKS.md` | Links externos por Component, popover JS y warmup. | CONTRACT DESIGN |

## Checkpoint CURRENT

```text
moragaga/atlanticus@59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
parent: 1302fefdf046b1cef7beed594e832f9a7a181a06
```

## Hito vigente

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Contrato CURRENT

```text
ManagerModule
├── source_key
├── source_service
├── source_reader_service
├── projection_service
├── draft_validation_service
└── source_history_service | None
```

Manager ya no declara:

```text
workflow_service
exact_source_reader_service
exact_source_history_service
exact_source_workflow_service
exact_projection_service
```

## Source

```text
SourceReaderWorkflow
SourcePublicationWorkflow
SourceHistoryWorkflow
```

Transportan directamente contratos de `source/core`.

## Projection

Manager usa directamente:

```text
ProjectionStatus
ProjectionTarget
ProjectionExecutionResult
```

No existe contrato Projection legacy paralelo.

## Workspace

```text
ManagerWorkspace schema 2
BASE = SourceSnapshot
revision = local payload identity
```

## Qualification

```text
web/capabilities/manager
54 passed
```

Full Web/ADA en este checkpoint: **UNVERIFIED**.

## Consumers pendientes

Cada uno se cierra por separado:

```text
Navigation      PLANNED / NEXT
Tools           PLANNED
KPI Configuration PLANNED
KPI Definition  PLANNED
```

No reintroducir legacy en Manager para facilitar esos cutovers.
