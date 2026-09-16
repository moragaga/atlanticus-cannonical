# Manager — Canonical Index

Estado: **CURRENT / GENERIC CORE + NAVIGATION + USERS CLOSED / ADA CONSUMERS MIGRATING**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como capability independiente. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar y navegación administrativa. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION y contrato genérico único. | CURRENT |
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

## Consumers cerrados

```text
Navigation
CLOSED / VERIFIED / CURRENT

Users Manager composition
CLOSED / VERIFIED / CURRENT
```

## Tools domain contract

```text
TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Tools ya usa Source/Projection genéricos directamente, pero su consumer dentro de `ada-configuration-manager` aún no fue cortado.

Esto es intencional.

## Regla de consumer adoption

Todo consumer Manager final debe usar el contrato genérico directamente.

FORBIDDEN:

```text
adapter Manager-specific para contrato viejo
double routing
revision-string lifecycle
revision -> ProjectionTarget reconstruction
alias dentro del dominio para sostener imports viejos
```

No es requisito mantener `ada-configuration-manager` ejecutable mientras sus dominios Source/Projection todavía se están migrando.

## Frentes pendientes

```text
KPI Configuration Source/Projection       PLANNED / NEXT
KPI Definition Source/Projection          PLANNED
Tools Manager consumer                    PLANNED
ADA Configuration Manager final cutover   BLOCKED
Global regression                         BLOCKED
```

El próximo incremento no es un cambio de Manager.

Debe inspeccionar sólo KPI Configuration usando Tools CURRENT como referencia estructural.
