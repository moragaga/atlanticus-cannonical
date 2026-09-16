# Manager — Canonical Index

Estado: **CURRENT / GENERIC CORE + NAVIGATION + USERS CONSUMERS CLOSED**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como capability independiente. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar y navegación administrativa. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION y contrato genérico único. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Herramienta, Component/Subcomponent, KPI y alarmas. | FROZEN/CURRENT |
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

## Consumers cerrados

```text
Navigation
CLOSED / VERIFIED / CURRENT

Users Manager composition
CLOSED / VERIFIED / CURRENT

Users Configuration clean cutover
CLOSED / VERIFIED / CURRENT
```

Users no debe reintroducir familia `Exact*`, revision lifecycle ni schema legacy runtime.

## Regla de consumer adoption

Todo consumer Manager debe usar el contrato genérico directamente.

FORBIDDEN:

```text
adapter Manager-specific para contrato viejo
double routing
revision-string lifecycle
revision -> ProjectionTarget reconstruction
schema fallback para conservar consumer viejo
```

## Qualification observada

```text
Users scoped Ruff
PASS

Users scoped pytest
99 passed

Web global
545 passed
7 skipped
0 failed

git diff --check HEAD^..HEAD
PASS

git status --short
CLEAN
```

## Consumers pendientes no revalidados

```text
Tools             PLANNED / NEXT
KPI Configuration PLANNED
KPI Definition    PLANNED
```

No asumir que requieren el mismo cambio.

El próximo incremento debe inspeccionar sólo Tools.
