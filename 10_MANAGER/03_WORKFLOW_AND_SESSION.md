# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / ROOT PROJECTION CUTOVER CLOSED / EXACT-SOURCE BOUNDARY CLOSED**

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

El modelo canónico distingue:

```text
BASE
    snapshot Source exacto observado al establecer el workspace

SOURCE
    current durable autoritativo, independiente del workspace

WORKSPACE
    estado editable local + revision propia

PROJECTION
    active projection de una Source release exacta
```

Estado:

```text
root Projection action cutover          CLOSED / VERIFIED / CURRENT
generic exact-source boundary           CLOSED / VERIFIED / CURRENT
legacy publication workflows            CURRENT
Users exact-source workflow wiring      PLANNED
browser persistence global              PLANNED
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
- no se degrada release/token a una revisión textual;
- el protocolo es opt-in;
- no reemplaza `ConfigurationLifecycleWorkflow`;
- un workflow legacy puede seguir sin implementar exact-source.

Coordinator:

```text
get_exact_source_snapshot(...)
publish_draft_exact(...)
```

`publish_draft_exact(...)`:
1. resuelve exclusivamente workflows que implementen `ExactSourcePublicationWorkflow`;
2. aplica autorización de publication;
3. relee current snapshot;
4. compara el value object completo con el snapshot esperado;
5. si difiere, falla con `ManagerSourceConflictError`;
6. invoca el workflow con el snapshot exacto;
7. si el workflow falla, relee Source;
8. si Source cambió, adjudica el fallo como conflict;
9. si Source no cambió, propaga el error original.

La garantía CAS final sigue perteneciendo a Source/workflow; el precheck de Manager no sustituye `ConcurrencyToken`.

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

La existencia del protocolo exact-source refina, pero no elimina, estos contratos.

## Session

Invariantes:
- primera visita hidrata desde Source cuando no existe workspace recuperable;
- navegación interna reutiliza working state;
- paginación/filtro local no relee Source;
- no hidratar preventivamente todo Manager;
- una BASE stale nunca autoriza sobrescribir automáticamente el WORKSPACE;
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
- el workspace/draft conserva `SourceSnapshot`;
- publication usa el token correspondiente;
- `basis_release` puede conservar la base original;
- Manager puede detectar stale snapshot antes del workflow;
- Source conserva el CAS final.

No implementar merge automático sin contrato de dominio.

## Source -> Projection

```text
ProjectionTarget =
    SourceKey
    +
    SourceReleaseRef
```

La ejecución puede proyectar una release seleccionada aunque Source avance.

## Checkpoints

Root exact Projection handoff:

```text
moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
```

Generic exact-source publication boundary:

```text
moragaga/atlanticus@9342769a626c39d1f7f860f81e051e2ef1300620
```

## Multi-module projection bootstrap

Permanece PLANNED.

No fue parte de este cierre.
