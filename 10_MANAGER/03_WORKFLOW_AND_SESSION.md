# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / GENERIC CONSUMER CUTOVER CLOSED**

## Flujo conceptual

```text
WORKSPACE
→ validate
→ verify Source
→ publish Source
→ project
→ history/preview
```

Guardar WORKSPACE no equivale a publicar Source.

## BASE / SOURCE / WORKSPACE / PROJECTION

```text
BASE
    SourceSnapshot observado al establecer/rebasar el workspace

SOURCE
    current durable autoritativo, independiente del workspace

WORKSPACE
    payload editable local + revision local + BASE

PROJECTION
    active projection de una Source release exacta
```

## ManagerModule

Contrato vigente:

```text
source_key
source_service
source_reader_service
projection_service
draft_validation_service
source_history_service | None
```

No existe routing alternativo exact/legacy.

## Source contracts

```text
SourceReaderWorkflow
    load_current_source() -> SourceReadResult

SourcePublicationWorkflow
    get_source_snapshot() -> SourceSnapshot
    publish_draft(payload, expected_source_snapshot) -> SourcePublicationResult

SourceHistoryWorkflow
    list_history(limit) -> HistoryPage
    load_history_release(release_ref: SourceReleaseRef) -> SourceHistoryReadResult
```

Invariantes:

- Source identity no se reduce a revision strings;
- publication conserva `SourceSnapshot`;
- conflicto se determina por release identity;
- History conserva `SourceReleaseRef`;
- History read devuelve la release solicitada.

## Projection contract

Manager transporta `ProjectionTarget` completo.

Invariantes:

- Manager no reconstruye target desde revision;
- `ProjectionTarget` conserva `SourceKey`, release exacta y dependencias;
- un target de otro `source_key` es inválido para el módulo;
- retry no cambia silenciosamente el target seleccionado.

## Workspace

`ManagerWorkspace` mantiene identidad local del payload separada de Source identity.

El Configuration Manager CURRENT usa `ManagerWorkspaceBridge`.

El bridge:

```text
read browser document
→ parse ManagerWorkspace
→ verify owner
→ expose payload

write payload
→ existing workspace.with_payload(...)
or
→ ManagerWorkspace.create(..., base=current SourceSnapshot)
```

No crea identidad Source desde una revision local.

## Configuration Manager adoption

Estado:

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT
```

El consumer final publicado compone workflows que satisfacen directamente los contratos genéricos.

Para Navigation, Tools, KPI Configuration y KPI Definition existen workflows de Source y Draft Validation en el composition package.

Users usa la composición Users Manager CURRENT.

Es legítimo que una misma instancia implemente Source reader/publication/history y se registre bajo service keys diferentes; esto no reintroduce `workflow_service` como contrato Manager.

## Contrato removido

SUPERSEDED / REMOVED:

```text
ConfigurationLifecycleWorkflow
ExactSourceReaderWorkflow
ExactSourcePublicationWorkflow
ExactSourceHistoryWorkflow
ExactProjectionWorkflow
workflow_service
exact_source_* services
exact_projection_service
expected_source_revision
source_revision como identidad ejecutable
revision -> ProjectionTarget reconstruction
```

## Runtime local

CURRENT para smoke/manual validation:

```text
LocalSourceStore
InProcessProjectionStore
```

La composición local incluye Users, Navigation, Tools, KPI Configuration y KPI Definition.

Esto demuestra composición ejecutable local, no E2E productivo.

## Qualification del cierre

Observado:

```text
static checks
PASS

local page boot
PASS
```

No observado:

```text
full behavioral E2E
full package regression
Storage/Cosmos E2E
```

## Siguiente frontera

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
PLANNED / NEXT
```

No inventar una nueva familia de workflows para resolver UI.

Cualquier contrato sospechoso debe contrastarse primero con este contrato CURRENT y con su implementación real.
