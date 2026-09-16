# Manager — Canonical Index

Estado: **CURRENT / GENERIC CORE + NAVIGATION CONSUMER CLOSED**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como aplicación/capability independiente y ownership de shell. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home `/manager`, sidebar, registry y navegación administrativa. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION y contrato genérico único. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Herramienta, Component/Subcomponent, KPI y alarmas. | FROZEN/CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection consumido por Manager genérico y consumer adoption. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual vs qualification visual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuentes/checkpoints/evidencia del Manager actual. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Primera instalación, bypass y acceso pre-Manager. | CURRENT DIRECTION |
| `09_ADA_COMPONENT_LINKS.md` | Links externos por Component, popover JS y warmup. | CONTRACT DESIGN |

## Checkpoint CURRENT

```text
moragaga/atlanticus@d34cda3838a67907728b382e238f0178f9f1a64e
parent: 59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
```

## Hitos vigentes

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
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

Manager no declara `workflow_service`, `exact_source_*` ni `exact_projection_service`.

## Navigation consumer CURRENT

```text
web/compositions/navigation-manager
```

Registra:

```text
navigation.configuration.source
navigation.configuration.projection
navigation.configuration.validation
```

Providers:

```text
Local: LocalSourceStore + LocalNavigationProjectionStore
Azure: BlobSourceStore + CosmosNavigationProjectionStore
```

## Qualification Navigation

```text
ruff scoped: PASS
pytest scoped: 102 passed
legacy forbidden scan: 0 results
```

## Bloqueo global observado

`users-manager` conserva consumo de contratos `Exact*` que Manager CURRENT ya no exporta.

Estado:

```text
USERS-MANAGER-ALIGNMENT-VALIDATION
PLANNED / NEXT
```

No se ha decidido todavía su implementación.

## Consumers pendientes no revalidados

```text
Tools             PLANNED
KPI Configuration PLANNED
KPI Definition    PLANNED
```

No reintroducir legacy en Manager ni Navigation para facilitar esos frentes.
