# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / USERS EXACT MANAGER LIFECYCLE CLOSED**

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

## BASE / SOURCE / WORKSPACE / PROJECTION

```text
BASE
    exact SourceSnapshot observado al establecer/rebasar el workspace

SOURCE
    current durable autoritativo, independiente del workspace

WORKSPACE
    payload editable local + revision propia + BASE

PROJECTION
    active projection de una Source release exacta
```

Guardar WORKSPACE no equivale a publicar Source.

## Capability model

Un `ManagerModule` puede declarar de forma independiente:

```text
workflow_service
draft_validation_service
exact_source_reader_service
exact_source_history_service
exact_source_workflow_service
exact_projection_service
```

Para un módulo exacto, `workflow_service` puede ser `None`.

No existe fallback silencioso desde un exact service declarado hacia legacy.

## Exact Source contracts

```text
ExactSourceReaderWorkflow
    load_current_source_exact() -> ExactSourceReadResult

ExactSourcePublicationWorkflow
    get_source_snapshot() -> SourceSnapshot
    publish_draft_exact(
        payload,
        expected_source_snapshot,
    ) -> ExactSourcePublicationResult

ExactSourceHistoryWorkflow
    list_history_exact(limit) -> HistoryPage
    load_history_release_exact(
        release_ref: SourceReleaseRef,
    ) -> ExactSourceHistoryReadResult
```

Invariantes:

- `ExactSourceReadResult` tiene payload exactamente cuando Source existe;
- payload se copia defensivamente;
- publication conserva `PublishResult` tipado;
- expected Source se transporta como `SourceSnapshot`;
- History conserva `SourceReleaseRef`;
- History read debe devolver la misma release solicitada;
- no reducir exact Source identity a strings legacy.

## Exact Projection contract

```text
ExactProjectionWorkflow
    get_status() -> projection.core.ProjectionStatus
    get_current_projection_target() -> ProjectionTarget | None
    project(target) -> projection.core.ProjectionExecutionResult
```

Manager conserva el status core:

```text
ProjectionStatus
├── alignment
├── source_current_release
└── projected_source_release
```

No sintetiza:

- actor;
- projection revision;
- projection audit timestamp.

## Coordinator routing

`ManagerProjectionCoordinator` decide por capability declarada:

- `get_status` usa exact Projection si existe;
- `get_current_projection_target` usa exact Projection si existe;
- `project` usa exact Projection si existe;
- validation usa `draft_validation_service`;
- exact Source read usa `exact_source_reader_service`;
- exact Source publication usa `exact_source_workflow_service`;
- exact History usa `exact_source_history_service`.

Legacy `ConfigurationLifecycleWorkflow` permanece sólo para módulos todavía no migrados.

## Status presentation

Exact status browser state conserva:

```text
source_release_id
source_published_at_utc
projected_source_release_id
projected_source_published_at_utc
```

Nunca los llama `source_revision`.

History se carga en un bloque independiente. Una ausencia/falla de History no degrada un status exacto válido.

## Manager exact workspace

`ManagerExactWorkspaceController`:

- carga Source current exacto;
- valida que el browser workspace pertenezca al principal;
- detecta local work;
- valida;
- verifica Source;
- publica exacto;
- rebasa tras publicación exitosa;
- permite reemplazar workspace desde Source current;
- permite reemplazar sólo payload local.

Force publication no forma parte del camino exacto Users.

## Users CURRENT

En ADA Configuration Manager:

```text
Users ManagerModule
    workflow_service               = None
    draft_validation_service       = USERS_DRAFT_VALIDATION_SERVICE
    exact_source_reader_service    = USERS_EXACT_SOURCE_READER_SERVICE
    exact_source_history_service   = USERS_EXACT_SOURCE_HISTORY_SERVICE
    exact_source_workflow_service  = USERS_EXACT_SOURCE_WORKFLOW_SERVICE
    exact_projection_service       = USERS_EXACT_PROJECTION_SERVICE
```

Service registration:

- validation se compone desde users-manager;
- reader se compone sobre `UsersProfilesAdministrationService`;
- History se compone sobre `UsersProfilesAdministrationService`;
- publication se compone sobre `UsersProfilesAdministrationService`;
- Projection se inyecta ya compuesta mediante dependencies.

`UsersManagerWorkflowAdapter` está removido.

## Users History CURRENT

History exacto usa:

```text
HistoryPage
SourceReleaseSummary
SourceReleaseRef
ExactSourceHistoryReadResult
```

La UI serializa para Dash:

```text
release_id
published_at_utc
```

Los dos campos reconstruyen `SourceReleaseRef`; no constituyen `revision`.

`build_users_history_preview` consume `UsersProfilesConfiguration`.

## Load historical as work

Cargar una release histórica:

1. lee exactamente `SourceReleaseRef`;
2. muestra preview canónico;
3. al elegir cargarla, aplica su payload al workspace local;
4. preserva BASE current;
5. invalida validation/source verification previas;
6. deja cambios locales;
7. requiere validate → verify → publish para crear una release nueva.

Si no existe workspace local, primero carga Source current para establecer BASE.

No repunta Source current.

## Concurrencia

Exact Source:

- workspace conserva `SourceSnapshot`;
- verify compara workspace BASE contra current exacto;
- publish usa exact snapshot verificado;
- coordinator detecta stale antes del workflow;
- Source conserva CAS final.

No implementar merge automático sin contrato de dominio.

## Source -> Projection

```text
ProjectionTarget =
    SourceKey
    +
    SourceReleaseRef
```

La ejecución puede proyectar una release seleccionada aunque Source avance.

## Legacy restante

Para módulos no migrados pueden seguir existiendo:

- `ProjectionStatus.source_revision`;
- `SourcePublicationResult.source_revision`;
- `SourceVerificationResult.source_revision`;
- `RevisionHistoryWorkflow`;
- `publish_draft(... expected_source_revision: str | None)`.

Esos strings no son Source exact identity.

Actualmente Navigation/Tools/KPI/KPI Definitions tienen además un gap Projection con el contrato Manager vigente.

## Qualification

Current checkpoint:

```text
384a68fe8fa42263623c95d1d132af2ca54574c8
```

Qualification:

```text
focused Manager + Users Configuration + users-manager  238 passed
full ADA                                               56 passed / 4 failed
```

Los 4 failures pertenecen a adapters Projection legacy no-Users.

## Browser persistence

`dcc.Store(memory)` = estado activo de sesión Dash.

IndexedDB general permanece PLANNED.

SourceStore/Blob sigue siendo autoridad durable publicada.

## Siguiente foco

```text
ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT
```

No reabrir Users exact lifecycle.
