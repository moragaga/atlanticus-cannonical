# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

Corte de implementación verificado para este cierre:

```text
moragaga/atlanticus@59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
parent: 1302fefdf046b1cef7beed594e832f9a7a181a06
```

Este documento actualiza únicamente el estado que cambió o fue revalidado durante el cutover genérico de Manager. Los dominios no inspeccionados en este cierre conservan su estado canónico anterior y no se consideran revalidados por este checkpoint.

## Resumen del hito

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER      CLOSED / VERIFIED / CURRENT

MANAGER-EXACT-SOURCE-BOUNDARY                  SUPERSEDED AS CURRENT CONTRACT
MANAGER-EXACT-ONLY-MODULE-CONTRACT             SUPERSEDED AS CURRENT CONTRACT
MANAGER-EXACT-PROJECTION-BOUNDARY               SUPERSEDED AS CURRENT CONTRACT
MANAGER-EXACT-SOURCE-WORKSPACE-CAPABILITIES    SUPERSEDED AS CURRENT CONTRACT

NAVIGATION-MANAGER-GENERIC-CONSUMER-CUTOVER    PLANNED
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER         PLANNED
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER    PLANNED
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER PLANNED

MANAGER-CONSUMER-GLOBAL-QUALIFICATION           BLOCKED
```

Los hitos `MANAGER-EXACT-*` se preservan como evidencia histórica de evolución, pero ya no describen el contrato vigente.

## Manager CURRENT

`ManagerModule` declara una sola familia de servicios:

```text
source_key
source_service
source_reader_service
projection_service
draft_validation_service
source_history_service | None
```

No existen en el contrato vigente de Manager:

```text
workflow_service
exact_source_reader_service
exact_source_history_service
exact_source_workflow_service
exact_projection_service
```

## Source contracts de Manager

Manager expone contratos genéricos:

```text
SourceReaderWorkflow
    load_current_source() -> SourceReadResult

SourcePublicationWorkflow
    get_source_snapshot() -> SourceSnapshot
    publish_draft(payload, expected_source_snapshot) -> SourcePublicationResult

SourceHistoryWorkflow
    list_history(limit) -> HistoryPage
    load_history_release(SourceReleaseRef) -> SourceHistoryReadResult
```

Invariantes:

- `SourceSnapshot` es la identidad de BASE observada por el workspace.
- `SourceReadResult` contiene payload exactamente cuando Source existe.
- payload se copia defensivamente.
- History conserva `SourceReleaseRef`.
- History read debe devolver la misma release solicitada.
- no se reduce Source identity a revision strings.

## Projection contract de Manager

Manager consume directamente `projection/core`:

```text
get_status(source_key) -> ProjectionStatus
select_current_target(source_key) -> ProjectionTarget | None
project(ProjectionTarget) -> ProjectionExecutionResult
```

Invariantes:

- `ProjectionTarget = SourceKey + SourceReleaseRef`.
- Manager no reconstruye target desde una revision textual.
- `project(...)` recibe el target ejecutable completo.
- el target debe pertenecer al `source_key` del módulo.
- no existe un modelo Projection legacy paralelo dentro de Manager.

## Workspace CURRENT

`ManagerWorkspace` schema vigente:

```text
schema_version = 2
owner_subject_id
revision
base_payload_revision
saved_at_utc
source_snapshot
payload
```

Semántica:

- `revision` identifica únicamente el payload local.
- `base_payload_revision` identifica la BASE local del payload.
- `source_snapshot` conserva la BASE Source.
- local revision no es Source release identity.
- parser schema `2` no adapta schema anterior.
- conflicto Source se determina por release identity (`snapshot.current`), no por cambio aislado de concurrency token.
- publish vuelve a consultar current y pasa el snapshot/token fresco al workflow cuando la release no cambió.
- publicación exitosa rebasa el workspace al snapshot publicado.

## Lifecycle CURRENT

Existe una sola ruta de lifecycle Manager.

La separación conceptual permanece:

```text
WORKSPACE
→ validate
→ verify Source
→ publish Source
→ project
→ history/preview
```

No existe `resolve_exact_source_lifecycle`.

No existe fallback exact→legacy.

## Eliminación legacy verificada en Manager

Removidos del Manager productivo y de su espejo comentado:

```text
exact_source.py
exact_projection.py
exact_workspace.py
web/exact_workspace.py
```

También se retiraron tests cuyo único contrato era la ruta exact/legacy anterior o estructura visual no contractual.

## Qualification del cierre

VERIFIED en el workspace del usuario:

```text
web/capabilities/manager
54 passed
0 failed
```

También está verificado en GitHub que `59fcd3e...` contiene el cutover y que su parent es `1302fefd...`.

No se presenta como evidencia vigente de `59fcd3e...`:

```text
238 passed
56 passed / 4 failed
```

Esos conteos pertenecen a checkpoints anteriores.

## OPEN relevante

El cutover de Manager core no demuestra que todos sus consumidores ya estén alineados.

Permanecen abiertos, cada uno como incremento independiente:

1. Navigation.
2. Tools.
3. KPI Configuration.
4. KPI Definition.
5. cualquier otro `ManagerModule` real que el árbol de `atlanticus:main` revele.

Para cada consumidor debe verificarse:

- construcción de `ManagerModule`;
- servicios registrados;
- Source reader/publication/history;
- Projection status/target/project;
- callbacks/layout específicos;
- tests propios;
- ausencia de legacy adapters/shims/aliases.

## Qualification todavía UNVERIFIED

- full Web suite en `59fcd3e...`;
- full ADA suite en `59fcd3e...`;
- suites de Navigation/Tools/KPI Configuration/KPI Definition contra el contrato nuevo;
- Docker E2E;
- CI remoto adicional;
- qualification global Python 3.14.7/Trixie;
- scan global del repositorio que demuestre ausencia total de residuos legacy fuera de Manager.

## Siguiente frontera

Un solo foco por chat.

```text
NEXT
Navigation Manager generic consumer cutover
```

No mezclar Tools, KPI Configuration ni KPI Definition en ese chat.
