# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / GENERIC CUTOVER CLOSED**

## Flujo conceptual

Manager separa configuración editable y workflow administrativo.

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
    publish_draft(
        payload,
        expected_source_snapshot,
    ) -> SourcePublicationResult

SourceHistoryWorkflow
    list_history(limit) -> HistoryPage
    load_history_release(
        release_ref: SourceReleaseRef,
    ) -> SourceHistoryReadResult
```

Invariantes:

- `SourceReadResult` tiene payload exactamente cuando Source existe;
- payload se copia defensivamente;
- publication conserva `PublishResult`;
- expected Source se transporta como `SourceSnapshot`;
- History conserva `HistoryPage` y `SourceReleaseRef`;
- History read debe devolver la misma release solicitada;
- Source identity no se reduce a strings de revisión.

## Projection contract

Manager consume el servicio genérico de Projection:

```text
get_status(source_key) -> ProjectionStatus
select_current_target(source_key) -> ProjectionTarget | None
project(target: ProjectionTarget) -> ProjectionExecutionResult
```

El coordinator:

- valida autorización;
- valida `SourceKey`;
- transporta `ProjectionTarget` completo;
- no reconstruye target desde una revision;
- no adapta resultados a un modelo Manager paralelo.

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

`revision` y `base_payload_revision` son identidad local del payload.

No son Source release identity.

## Source verification

`ManagerSourceVerification` conserva:

```text
workspace_revision
base: SourceSnapshot
source: SourceSnapshot
checked_at_utc
```

`matches` compara:

```text
base.current == source.current
```

Un cambio aislado del concurrency token no equivale a una nueva Source release.

## Publication concurrency

Antes de publicar:

1. coordinator valida que `expected_source_snapshot.source_key` sea el del módulo;
2. relee Source current;
3. si cambió `current` release → conflict;
4. si la release es la misma, usa el snapshot current fresco;
5. el workflow recibe ese snapshot con el token actual;
6. Source conserva el CAS autoritativo final.

Si publication falla y una reread detecta cambio de release, Manager expone conflicto.

## Lifecycle

Existe una sola función de lifecycle:

```text
resolve_manager_lifecycle
```

`resolve_exact_source_lifecycle` fue removido.

## History

History es opcional por módulo.

Cuando existe:

- lista publicaciones Source reales;
- conserva `SourceReleaseRef`;
- preview no cambia current;
- cargar historical reemplaza payload local;
- BASE current se conserva;
- validation y verification previas quedan inválidas;
- volver a publicar crea una release nueva.

## Projection selection

El target actual se obtiene server-side desde el projection service.

El browser no construye identidad ejecutable a partir de strings.

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

## Consumer boundary

Manager core no contiene excepciones especiales para Navigation, Tools, KPI Configuration o KPI Definition.

Cada consumer debe implementar/registrar el contrato genérico directamente.

## Qualification

```text
59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
web/capabilities/manager: 54 passed
```

Full consumer integration permanece UNVERIFIED.
