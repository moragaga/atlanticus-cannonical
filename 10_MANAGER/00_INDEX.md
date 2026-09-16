# Manager — Canonical Index

Estado: **CURRENT GENERIC CORE / DOMAIN CONTRACTS CLOSED / ADA CONFIGURATION CONSUMER NEXT**

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
```

## Configuration Manager consumer mismatch

`scopes/ada/web/application/ada-configuration-manager` todavía usa contratos anteriores.

Verificado en el checkpoint CURRENT:

```text
workflow_service
exact_source_*
ExactProjectionWorkflow
ToolLifecycleServices
KpiConfigurationServices
KpiDefinitionServices
KpiDefinitionAuthorityProvider
revision-string workflow adapters
expected_source_revision
```

Además intenta importar nombres `create_users_manager_exact_source_*` que ya no forman parte del package Users Manager CURRENT.

Esto no reabre Manager core ni los dominios cerrados.

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

## Siguiente frontera

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

El objetivo es cortar el consumer completo una sola vez, no migrar Definition aisladamente dentro del Manager.

Después:

```text
MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED until final cutover
```
