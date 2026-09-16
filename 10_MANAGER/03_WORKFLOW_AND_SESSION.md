# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / GENERIC CUTOVER CLOSED**

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

Manager coordina Projection mediante el contrato genérico y transporta `ProjectionTarget` completo.

Invariantes:

- Manager no reconstruye target desde revision;
- `ProjectionTarget` conserva `SourceKey`, release exacta y dependencias;
- un target de otro `source_key` es inválido para el módulo;
- retry no cambia silenciosamente el target seleccionado.

## Workspace

`ManagerWorkspace` schema `2` conserva:

```text
owner_subject_id
revision
base_payload_revision
saved_at_utc
source_snapshot
payload
```

`revision` y `base_payload_revision` son identidad local del payload, no Source release identity.

`build_workspace_revision(payload)` pertenece a esta identidad local. No sustituirlo por digest/revision privado del dominio.

## Navigation adoption

Navigation implementa el contrato genérico sin una segunda familia de workflows.

Estado:

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Users adoption

Users Manager composition también fue cortado al contrato genérico.

Package CURRENT exporta:

```text
UsersManagerDraftValidationWorkflow
UsersManagerSourceWorkflow
create_users_manager_draft_validation_workflow
create_users_manager_source_workflow
```

No exporta la familia anterior `create_users_manager_exact_source_*`.

Estado:

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

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

## Consumer mismatch CURRENT

`scopes/ada/web/application/ada-configuration-manager` todavía referencia la familia anterior.

Verificado:

```text
composition.py imports create_users_manager_exact_source_* names
composition.py constructs ManagerModule with exact_source_* / workflow_service
workflows.py translates revision-string lifecycles
```

Eso contradice el contrato Manager CURRENT y debe resolverse en el consumer, no reabriendo Manager core ni Users.

## Siguiente frontera

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

No inventar una tercera familia de workflows. Componer directamente servicios que satisfagan los contratos genéricos CURRENT.

## Qualification

La qualification del Manager core pertenece a sus checkpoints cerrados previos.

Este cierre no ejecutó una regression final de `ada-configuration-manager` sobre `ef3f0a44...`.

Estado:

```text
final Configuration Manager runtime
UNVERIFIED / BLOCKED until consumer cutover
```
