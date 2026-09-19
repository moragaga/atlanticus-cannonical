# Manager — Canonical Index

Estado: **CURRENT GENERIC CORE / AUTHORIZATION CLEANUP OPEN**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como capability independiente. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar y navegación administrativa. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION y consumer final. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Herramienta, Component/Subcomponent y contrato Source/Projection CURRENT. | FROZEN/CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection consumido por Manager genérico. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuentes/checkpoints/evidencia. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Bootstrap separado de Manager Access y gap de autorización stale. | CURRENT DIRECTION / OPEN GAP |
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

Tools Source/Projection
CLOSED / VERIFIED / CURRENT

KPI Configuration Source/Projection
CLOSED / VERIFIED / CURRENT

KPI Definition Source/Projection
CLOSED / VERIFIED / CURRENT

ADA Configuration Manager final generic cutover
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

Users Manager composition no es CURRENT: fue removida cuando Users dejó de ser
Configuration Source.

## Configuration Manager CURRENT

Checkpoint global de referencia:

```text
moragaga/atlanticus@3eb46dac80f23d438774e3afa39999dc96f592d7
```

El consumer registra contratos genéricos separados por módulo:

```text
source
source-reader
source-history
projection
draft-validation
```

La surface CURRENT incluye:

```text
navigation
tools
kpis                 optional
kpi-definitions      optional
```

Users no es `ManagerModule`.

## Navigation Manager CURRENT

Navigation Manager puede recibir:

```text
profile_catalog_provider: NavigationProfileCatalogProvider | None
validators: tuple[NavigationProjectionValidator, ...]
```

Si hay `ProfileCatalog`, la misma validación referencial participa en draft y Projection.

Navigation Manager no obtiene perfiles desde Users ni ADA Access.

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
NavigationProfileOptionsProvider
profile_options_provider
projection_validators parameter name
```

No reintroducirlos para corregir UI o autorización.

## Gap CURRENT

`DefaultManagerAuthorizationPolicy` todavía contiene bypass:

```text
principal.is_local
OR
'administrator' in principal.profile_keys
```

ADA Configuration Manager conserva helpers equivalentes.

Estado:

```text
Manager authorization stale administrator/local semantics
OPEN / PROPOSED NEXT
```

No resolver sin inspeccionar runtime local y consumers.
