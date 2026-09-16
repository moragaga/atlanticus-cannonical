# Manager — Canonical Index

Estado: **CURRENT GENERIC CORE / ADA CONFIGURATION CONSUMER CURRENT / UI CLEANUP NEXT**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como capability independiente. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar y navegación administrativa. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION y consumer final. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Herramienta, Component/Subcomponent y contrato Source/Projection CURRENT. | FROZEN/CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection consumido por Manager genérico. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuentes/checkpoints/evidencia. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Primera instalación y acceso. | CURRENT DIRECTION |
| `09_ADA_COMPONENT_LINKS.md` | Links externos y warmup. | CONTRACT DESIGN |

## Contrato Manager CURRENT

```text
ManagerModule
├── source_key
├── source_service
├── source_reader_service
├── projection_service
├── draft_validation_service
└── source_history_service | None
```

Manager no declara:

```text
workflow_service
exact_source_*
exact_projection_service
expected_source_revision
```

## Consumers/contratos cerrados

```text
Navigation Manager adoption
CLOSED / VERIFIED / CURRENT

Users Manager composition
CLOSED / VERIFIED / CURRENT

Tools Source/Projection
CLOSED / VERIFIED / CURRENT

KPI Configuration Source/Projection
CLOSED / VERIFIED / CURRENT

KPI Definition Source/Projection
CLOSED / VERIFIED / CURRENT

ADA Configuration Manager final generic cutover
CLOSED / VERIFIED / CURRENT
```

## Configuration Manager CURRENT

Publicado en:

```text
moragaga/atlanticus@ee9a0401c7947f2bf61abc0a783dfa905443b6b1
```

El consumer registra contratos genéricos separados por módulo:

```text
source
source-reader
source-history
projection
draft-validation
```

El workspace de los editores se integra mediante `ManagerWorkspaceBridge` sobre `ManagerWorkspace`.

El package incluye un runtime local ejecutable con `LocalSourceStore` e `InProcessProjectionStore`.

## Legacy consumer removal

SUPERSEDED / REMOVED:

```text
ToolLifecycleServices
KpiConfigurationServices
KpiDefinitionServices
KpiDefinitionAuthorityProvider
NavigationConfigurationServices
ExactProjectionWorkflow
workflow_service
exact_source_*
expected_source_revision
revision-string workflow adapters
```

No reintroducirlos para corregir UI.

## Evidencia de cierre

```text
git diff --check
PASS

legacy scan
0 matches

compileall
PASS

local UI boot
PASS / manual smoke
```

Full behavioral E2E continúa UNVERIFIED.

## Siguiente frontera

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
PLANNED / NEXT
```

Después, en incrementos separados:

```text
ADA-CONFIGURATION-MANAGER-LOCAL-E2E
PLANNED

ADA-CONFIGURATION-MANAGER-STORAGE-COSMOS-E2E
PLANNED
```
