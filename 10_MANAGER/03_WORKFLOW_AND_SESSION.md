# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / ROOT PROJECTION CUTOVER CLOSED / EXACT-SOURCE BOUNDARY CLOSED / USERS EXACT-SOURCE COMPOSITION CLOSED**

## Flujo conceptual

Manager conserva separación entre configuración editable y workflow administrativo.

Estados/acciones conceptuales:
- working workspace;
- persisted browser workspace;
- validate;
- verify source;
- publish/save to Source;
- project;
- history/preview;
- conflict handling.

## BASE / SOURCE / WORKSPACE / PROJECTION

```text
BASE
    snapshot Source exacto observado al establecer el workspace

SOURCE
    current durable autoritativo, independiente del workspace

WORKSPACE
    estado editable local + revision propia + base local

PROJECTION
    active projection de una Source release exacta
```

Estado:

```text
root Projection action cutover                 CLOSED / VERIFIED / CURRENT
generic exact-source boundary                  CLOSED / VERIFIED / CURRENT
Users admin draft baseline semantics           CLOSED / VERIFIED / CURRENT
Users exact-source composition adapter         CLOSED / VERIFIED / CURRENT
Users productive exact-source service cutover  PLANNED
legacy publication workflows                   CURRENT
browser persistence global                     PLANNED
```

No crear segundo coordinator ni shim `SourceReleaseId <-> str`.

## Root Projection action

Contrato congelado:

```text
ConfigurationLifecycleWorkflow
    get_current_projection_target() -> ProjectionTarget | None
    project(target: ProjectionTarget) -> ProjectionExecutionResult

ProjectionExecutionResult
    target: ProjectionTarget
```

`ManagerProjectionCoordinator.project(...)` recibe un `ProjectionTarget` y lo entrega sin convertirlo a string ni releer current.

Browser state no es autoridad del target ejecutable.

## Exact-source publication boundary

Checkpoint:

```text
moragaga/atlanticus@9342769a626c39d1f7f860f81e051e2ef1300620
```

Manager define:

```text
@runtime_checkable
ExactSourcePublicationWorkflow

get_source_snapshot() -> SourceSnapshot

publish_draft_exact(
    payload: dict[str, object],
    expected_source_snapshot: SourceSnapshot,
) -> ExactSourcePublicationResult
```

Resultado:

```text
ExactSourcePublicationResult
├── source: PublishResult
├── audit: ProjectionAuditRecord
└── summary: tuple[ProjectionSummaryItem, ...]
```

Propiedades congeladas:
- `SourceSnapshot` se transporta como value object;
- `PublishResult` se conserva tipado;
- no se degrada release/token a revisión textual;
- protocolo opt-in;
- no reemplaza `ConfigurationLifecycleWorkflow`;
- workflow legacy puede seguir sin exact-source.

Coordinator:

```text
get_exact_source_snapshot(...)
publish_draft_exact(...)
```

`publish_draft_exact(...)`:
1. resuelve workflow exact-source;
2. aplica autorización;
3. relee current snapshot;
4. compara value object completo;
5. stale → `ManagerSourceConflictError`;
6. invoca workflow con snapshot exacto;
7. si workflow falla, relee Source;
8. si Source cambió, adjudica conflict;
9. si Source no cambió, propaga error original.

CAS final sigue perteneciendo a Source/workflow.

## Users admin draft baseline semantics

Checkpoint:

```text
moragaga/atlanticus@567e1a12c862b46dfd7f4ec75c3be750c95bbd54
```

`UsersProfilesAdminDraft` schema `2` agrega:

```text
revision
base_payload_revision
source_snapshot
```

Invariantes:
- create nace clean;
- dirty = `revision != base_payload_revision`;
- edit preserva BASE + `SourceSnapshot`;
- rebase adopta nuevo exact `SourceSnapshot` y convierte revision actual en nueva BASE;
- schema 1 no se acepta;
- local revision no es Source identity.

Guardar WORKSPACE sigue sin equivaler a publicar Source.

## Users exact-source composition

Checkpoint:

```text
moragaga/atlanticus@7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98
```

Implementado:

```text
web/compositions/users-manager
└── UsersManagerExactSourceWorkflow
```

Responsabilidades:
- `get_source_snapshot()` delega a `UsersProfilesAdministrationService`;
- `publish_draft_exact(...)` parsea `UsersProfilesConfiguration`;
- actor proviene de `UsersAuditActorProvider`;
- publication delega al backend Users exact-source;
- resultado conserva `PublishResult`;
- audit timestamp proviene de la release publicada.

No responsabilidades:
- no almacena draft;
- no hace rebase;
- no proyecta;
- no conoce Dash/IndexedDB;
- no registra servicios del host.

Dirección de dependencias:

```text
Manager                     Users Configuration
   ↑                               ↑
   └── compositions/users-manager ─┘
```

## Productive service cutover

No está cerrado.

El host ADA Configuration Manager todavía registra:

```text
UsersManagerWorkflowAdapter(dependencies.users)
```

El adapter productivo vigente usa:
- `UsersConfigurationCatalog`;
- `publish_draft(payload, expected_source_revision: str | None)`.

Por tanto:

```text
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER
PLANNED
```

No confundir “composition adapter disponible” con “publication action productiva migrada”.

## Qué permanece legacy

Permanecen vigentes para workflows no migrados:
- `ProjectionStatus.source_revision`;
- `ProjectionStatus.active_source_revision`;
- `SourcePublicationResult.source_revision`;
- `SourceVerificationResult.source_revision`;
- `publish_draft(... expected_source_revision: str | None)`;
- history/load revision textual.

Esos strings:
- no son `SourceReleaseId`;
- no son `SourceSnapshot`;
- no deben reinterpretarse mediante shim.

## Session

Invariantes:
- primera visita hidrata desde Source cuando no existe workspace recuperable;
- navegación interna reutiliza working state;
- paginación/filtro local no relee Source;
- no hidratar preventivamente todo Manager;
- BASE stale nunca autoriza sobrescribir automáticamente WORKSPACE;
- SOURCE se consulta independientemente para verificar conflicto.

## Browser WORKSPACE

Dirección global decidida:

```text
dcc.Store(storage_type="memory")
    = estado activo de sesión Dash

IndexedDB
    = persistencia browser del WORKSPACE

SourceStore / Blob
    = autoridad durable publicada

ProjectionStore
    = proyección activa durable
```

IndexedDB general permanece PLANNED.

Un dominio puede migrar su draft store actual al contrato canónico sin declarar implementado el WORKSPACE IndexedDB global.

## Draft / Workspace

Guardar WORKSPACE no equivale a publicar Source.

- WORKSPACE no genera Source release;
- sólo Source publication crea release;
- revision local del draft no es release identity;
- History durable contiene publicaciones reales, no autosaves.

## Concurrencia

Backend autoritativo aplica `ConcurrencyToken`.

Para exact-source:
- workspace/draft conserva `SourceSnapshot`;
- publication usa token correspondiente;
- `basis_release` preserva provenance/base según contrato del dominio;
- Manager puede detectar stale snapshot antes del workflow;
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

## Multi-module projection bootstrap

Permanece PLANNED.

No fue parte de este cierre.
