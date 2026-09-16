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

```text
get_status(source_key) -> ProjectionStatus
select_current_target(source_key) -> ProjectionTarget | None
project(target: ProjectionTarget) -> ProjectionExecutionResult
```

Manager no reconstruye target desde revision.

## Workspace

`ManagerWorkspace` schema `2`:

```text
owner_subject_id
revision
base_payload_revision
saved_at_utc
source_snapshot
payload
```

`revision` y `base_payload_revision` son identidad local del payload, no Source release identity.

## Navigation adoption

Navigation ya implementa el contrato anterior sin una segunda familia de workflows.

Un mismo workflow Navigation satisface las superficies Source reader/publication/history y se registra con una única service key Source.

Projection se registra directamente como servicio genérico.

Estado:

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
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

## Consumer mismatch observado

`web/compositions/users-manager` todavía consume nombres `Exact*`.

Esto contradice el contrato CURRENT de Manager, pero su solución no está decidida.

```text
USERS-MANAGER-ALIGNMENT-VALIDATION
PLANNED / NEXT
```

No reabrir Manager core para resolverlo.

## Qualification

```text
d34cda3838a67907728b382e238f0178f9f1a64e

Navigation + Manager scoped:
102 passed

Ruff scoped:
PASS

Full Web:
BLOCKED DURING COLLECTION BY users-manager
```
