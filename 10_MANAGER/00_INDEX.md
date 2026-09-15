# Manager — Canonical Index

Estado: **CURRENT / USERS EXACT MANAGER LIFECYCLE CLOSED**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como aplicación/capability independiente y ownership de shell. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home `/manager`, sidebar, registry y navegación administrativa. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | BASE/SOURCE/WORKSPACE, exact capabilities, Users lifecycle exacto, History y legacy residual. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Herramienta, Component/Subcomponent, KPI y alarmas. | FROZEN/CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection exact-release y adopción por Manager/Users. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual vs qualification visual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuentes/checkpoints/evidencia del Manager actual. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Primera instalación, bypass y acceso pre-Manager. | CURRENT DIRECTION |
| `09_ADA_COMPONENT_LINKS.md` | Links externos por Component, popover JS y warmup. | CONTRACT DESIGN |

## Checkpoint CURRENT

```text
moragaga/atlanticus@384a68fe8fa42263623c95d1d132af2ca54574c8
parent: b2254450b4543d2422ca8580357b9054b515cd6e
```

## Checkpoints preservados

```text
MANAGER-ROOT-CANONICAL-CUTOVER
CLOSED / VERIFIED / CURRENT

MANAGER-EXACT-SOURCE-BOUNDARY
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-EXACT-SOURCE-COMPOSITION
CLOSED / VERIFIED / CURRENT

ADMIN-UI-DRAFT-CUTOVER
CLOSED / VERIFIED / CURRENT

MANAGER-EXACT-PROJECTION-BOUNDARY
CLOSED / VERIFIED / CURRENT

USERS-EXACT-PROJECTION-STATUS-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-EXACT-SOURCE-HISTORY-BOUNDARY
CLOSED / VERIFIED / CURRENT

USERS-EXACT-SOURCE-HISTORY-HOST-UI-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-EXACT-MANAGER-LIFECYCLE
CLOSED / VERIFIED / CURRENT
```

## Users module CURRENT

```text
workflow_service               None
draft_validation_service       exact/canonical
exact_source_reader_service    exact
exact_source_history_service   exact
exact_source_workflow_service  exact
exact_projection_service       exact
```

Lifecycle:

```text
validate        EXACT
read            EXACT
publish         EXACT
status          EXACT
project         EXACT
history list    EXACT
history read    EXACT
history preview EXACT
history -> work EXACT
```

`UsersManagerWorkflowAdapter` ya no forma parte del host ni de su API pública.

## Límite legacy actual

Navigation, Tools, KPI y KPI Definitions siguen usando adapters legacy ADA.

El full ADA suite actual no está GREEN por una desalineación Projection de esos adapters con Manager vigente.

Estado:

```text
ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT
PLANNED / NEXT
```

No resolver esos fallos reintroduciendo legacy en Users.

## Qualification

```text
focused Manager + Users Configuration + users-manager   238 passed
full ADA                                                56 passed / 4 failed
```

Los cuatro failures fueron adjudicados al frente Projection legacy no-Users.

Full Web suite current checkpoint, Docker E2E, Python 3.14.7 y CI remoto permanecen UNVERIFIED.
