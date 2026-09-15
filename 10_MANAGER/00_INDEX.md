# Manager — Canonical Index

Estado: **CURRENT / ROOT PROJECTION CUTOVER CLOSED / EXACT-SOURCE BOUNDARY CLOSED / USERS EXACT-SOURCE COMPOSITION CLOSED**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como aplicación/capability independiente y ownership de header/shell. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home `/manager`, sidebar, registry y navegación administrativa. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | Workflow, sesión, BASE/SOURCE/WORKSPACE, exact-source y estado del cutover Users. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Herramienta, Component/Subcomponent, KPI y alarmas. | FROZEN/CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection exact-release y estado de migraciones consumidoras. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Qué debe probarse automáticamente y qué queda en qualification visual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuentes de decisión y checkpoints de implementación inspeccionados. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Primera instalación, bypass y acceso pre-Manager. | CURRENT DIRECTION |
| `09_ADA_COMPONENT_LINKS.md` | Links externos por Component, popover JS y warmup. | CONTRACT DESIGN |

## Checkpoints

Root Projection:

```text
MANAGER-ROOT-CANONICAL-CUTOVER
CLOSED / VERIFIED / CURRENT
moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
```

Generic exact-source publication boundary:

```text
MANAGER-EXACT-SOURCE-BOUNDARY
CLOSED / VERIFIED / CURRENT
moragaga/atlanticus@9342769a626c39d1f7f860f81e051e2ef1300620
```

Users exact-source composition adapter:

```text
USERS-MANAGER-EXACT-SOURCE-COMPOSITION
CLOSED / VERIFIED / CURRENT
moragaga/atlanticus@7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98
```

## Límite

La composition exact-source Users existe y está validada, pero el host ADA productivo todavía registra `UsersManagerWorkflowAdapter` legacy.

Por tanto:

```text
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER
PLANNED
```

No implica que todos los modelos administrativos legacy basados en `source_revision: str` hayan sido eliminados.
