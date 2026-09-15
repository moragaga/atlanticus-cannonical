# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / ROOT PROJECTION CUTOVER CLOSED / EXACT-SOURCE BOUNDARY CLOSED / USERS EXACT-SOURCE COMPOSITION CLOSED / USERS ADMIN UI CUTOVER CLOSED**

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
Users admin UI draft cutover                   CLOSED / VERIFIED / CURRENT
Users productive exact-source service cutover  PLANNED / NEXT CANDIDATE
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

## Users Admin UI draft cutover

Checkpoint:

```text
moragaga/atlanticus@d23bff025ab899367a8da1178dde5ab50806fe47
```

Active Users admin UI:

```text
UsersAdminWebContext
    administration: UsersProfilesAdministrationService

CATALOG_STORE_ID
    UsersProfilesConfiguration document

DRAFT_BASIS_STORE_ID
    UsersProfilesAdminDraft schema 2
```

Session invariants CURRENT para Users:
- load sin draft recuperable → `administration.create_draft(owner)` desde Source current;
- load de draft schema 2 válido y mismo owner → recupera draft;
- draft legacy/incompatible → no convierte; descarta y crea clean desde Source current;
- edit local modifica payload pero preserva BASE + exact `SourceSnapshot`;
- save draft = `basis.with_configuration(configuration)`;
- save local escribe draft/saved/basis stores, no Source;
- import legacy file transforma payload a canonical sobre la BASE existente;
- revision local sigue sin ser Source identity.

El layout usa `dcc.Store(storage_type="memory")`; IndexedDB general sigue PLANNED.

## Productive service cutover

No está cerrado.

El host ADA Configuration Manager todavía registra:

```text
UsersManagerWorkflowAdapter(dependencies.users)
```

El editor Users, en cambio, ya recibe:

```text
UsersProfilesAdministrationService
```

mediante `ConfigurationManagerDependencies.users_profiles_administration`.

Por tanto existe una frontera transitoria intencional:

```text
UI authoring Users       canonical schema 2
Manager Users workflow   legacy registration
```

No confundir “UI canónico” ni “composition exact-source disponible” con “publication action productiva migrada”.

Estado:

```text
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER
PLANNED / NEXT CANDIDATE
```

## Gap a verificar en el siguiente foco

VERIFIED:
- el workflow productivo Users registrado sigue legacy;
- Manager actual `0.3.15` exige `ProjectionTarget` en su lifecycle projection contract;
- adapters ADA observados todavía muestran incompatibilidades al ejecutar full suite con overlay Manager actual;
- lock ADA resuelve Manager `0.3.14` mientras el source inspeccionado es `0.3.15`;
- lock ADA resuelve Users Configuration `0.1.6` mientras el source inspeccionado es `0.1.9`.

INFERRED / TO VERIFY:
- el draft schema 2 compartido puede ser incompatible con callbacks legacy que todavía esperen `ManagerDraft` schema 1; verificar código activo antes de modificar.

UNVERIFIED:
- constructor físico externo que provee `users_profiles_administration` al host productivo;
- full ADA suite GREEN con Manager actual después de una alineación limpia de dependencies/contracts.

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

Invariantes generales:
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

El Users admin UI ya usa el contrato canónico en stores memory; esto no declara implementado IndexedDB global.

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
