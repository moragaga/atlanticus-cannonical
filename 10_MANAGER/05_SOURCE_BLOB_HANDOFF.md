# Manager — Source Blob Handoff

Estado: **SOURCE IMPLEMENTED / USERS EXACT MANAGER LIFECYCLE CLOSED / CONSUMER MIGRATION IN PROGRESS**

## Source productivo

Source productivo disponible:

```text
Azure Blob Storage
```

Local conserva semántica equivalente de desarrollo/QA.

SharePoint/Power Automate permanecen sólo donde existan consumidores legacy.

## Source Core

Source Core cubre:

- `SourceKey`;
- `SourceReleaseId`;
- `SourceReleaseRef`;
- immutable releases;
- manifest;
- `SourceStore`;
- current/concurrency;
- History;
- exact reads;
- integrity verification.

Blob cubre manifest como commit point, create-only first publish, conditional write, `ConcurrencyToken` opaco, recovery e integridad.

## History

Cada publicación Source es snapshot completo, autocontenido e inmutable.

`SourceReleaseId` no equivale a `content_hash`.

History durable contiene publicaciones reales, no autosaves.

## Restore

Restore publica una nueva release.

Nunca repunta current directamente a una release histórica.

Manager Users implementa este principio cargando el payload histórico como trabajo local sobre BASE current.

## No-op publish

`SourceStore.publish(...)` válido crea una nueva publicación.

Si una UI desea no-op funcional debe decidirlo antes de invocar Source.

## Concurrencia

Backend aplica la precondición autoritativa con `ConcurrencyToken`.

No existe `force=True`.

`basis_release` preserva base/provenance según el contrato de publication.

## Source -> Projection

```text
ProjectionTarget =
    SourceKey
    +
    SourceReleaseRef
```

`project(target)` lee la release exacta, no consulta current durante ejecución y conserva provenance exacto.

## Manager exact adoption

Manager CURRENT expone:

```text
ExactSourceReaderWorkflow
ExactSourcePublicationWorkflow
ExactSourceHistoryWorkflow
ExactProjectionWorkflow
```

No convierte `SourceReleaseRef`, `ConcurrencyToken`, `SourceSnapshot` ni `HistoryPage` a contratos legacy de revisión textual.

## Users checkpoint

Users Configuration implementa:

- Source codec/service;
- History y exact reads;
- multi-resource Users+Profiles release;
- canonical payload `UsersProfilesConfiguration`;
- exact Source→Projection service;
- Cosmos Projection schema `2` con lectura histórica schema `1`;
- exact provenance en Projection;
- CAS/idempotencia exact-target.

Users Manager implementa:

- canonical draft validation;
- exact Source reader;
- exact Source publication;
- exact Projection status/target/project;
- exact History list/read;
- canonical History preview;
- History load como local work.

Host ADA:

- no registra Users lifecycle legacy;
- registra servicios exactos separados;
- recibe Projection ya compuesta;
- no exporta `UsersManagerWorkflowAdapter`.

Estado:

```text
USERS-EXACT-MANAGER-LIFECYCLE
CLOSED / VERIFIED / CURRENT
```

## Exact History handoff

```text
SourceStore.query_history(...)
        ↓
HistoryPage
        ↓
UsersProfilesAdministrationService
        ↓
UsersManagerExactSourceHistoryWorkflow
        ↓
ManagerProjectionCoordinator
        ↓
preview / local workspace
```

La identidad se conserva como `SourceReleaseRef`.

No hay `SourceReleaseId -> revision` shim.

## Consumer migration todavía pendiente

Fuera del Manager Users ya migrado:

- Navigation/Tools/KPI/KPI Definitions Projection contract alignment;
- runtime canonical Users cutover;
- runtime exact-release provenance;
- otros consumidores administrativos legacy;
- resource topology físico canonical Users Projection;
- legacy deletion global;
- browser IndexedDB global;
- retiro efectivo SharePoint/Power Automate por consumidor.

## Qualification caveat

Current checkpoint:

```text
384a68fe8fa42263623c95d1d132af2ca54574c8
```

Focused Users/Manager suite: `238 passed`.

Full ADA: `56 passed / 4 failed`.

Los failures restantes pertenecen a adapters Projection legacy no-Users y no reabren el Source/History/Projection exacto de Users.

Docker E2E y wiring físico externo permanecen UNVERIFIED.
