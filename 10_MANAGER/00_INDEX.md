# Manager — Canonical Index

Estado: **CURRENT / GENERIC CORE + NAVIGATION + USERS MANAGER CONSUMERS**

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
```

Users Manager ya no debe reintroducir una familia `Exact*`.

## Users Configuration

El consumer Manager está alineado, pero la capability de configuración todavía no está cerrada porque el working tree local contiene compatibilidad schema v1.

```text
USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
IN PROGRESS

USERS-CLEAN-CUTOVER-COMPLETION
PLANNED / NEXT
```

La compatibilidad detectada debe eliminarse, no adaptarse.

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

Users scoped:

```text
ruff: PASS
pytest: 113 passed
```

Web global local:

```text
546 passed
7 skipped
```

No declarar Users Configuration CLOSED hasta eliminar schema v1 y repetir qualification.

## Consumers pendientes no revalidados

```text
Tools             PLANNED
KPI Configuration PLANNED
KPI Definition    PLANNED
```

No abrirlos en paralelo con `USERS-CLEAN-CUTOVER-COMPLETION`.
