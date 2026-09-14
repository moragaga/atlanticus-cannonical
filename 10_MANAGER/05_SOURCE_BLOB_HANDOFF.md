# Manager — Source Blob Handoff

Estado: **SOURCE IMPLEMENTED / ROOT PROJECTION CUTOVER CLOSED / EXACT-SOURCE BOUNDARY CLOSED / CONSUMER MIGRATION IN PROGRESS**

Fuente histórica:
`Atlanticus_ADA_Upgrade_Source_Blob_Trabajo_Pendiente.docx`
(10-09-2026)

## Cambio

Source productivo disponible:

```text
Azure Blob Storage
```

Local conserva semántica equivalente de desarrollo/QA.

Projection genérica usa handoff exact-release.

SharePoint y Power Automate permanecen sólo donde existan consumidores legacy.

## Estado Source

Implementado y validado:

```text
Source Core
Local Source
Blob Source
Projection exact-release Core
Manager root Projection exact-target transport
Manager exact-source publication boundary
```

Source Core cubre:
- release identity;
- release granularity;
- functional manifest;
- `SourceStore`;
- concurrencia/current;
- History;
- exact release reads;
- integrity verification.

Blob cubre:
- manifest como commit point;
- create-only first publish;
- conditional write;
- `ConcurrencyToken` público opaco;
- releases inmutables;
- recovery e integridad.

## Historial

Cada publicación Source es snapshot completo, autocontenido e inmutable.

`SourceReleaseId` no equivale a `content_hash`.

## Restore

Restore publica una nueva release.

Nunca repunta current directamente a una release histórica.

## No-op publish

`SourceStore.publish(...)` válido crea una nueva publicación.

Si UI desea no-op funcional debe decidirlo antes de invocar Source.

## Concurrencia

Backend aplica la precondición autoritativa con `ConcurrencyToken`.

No existe `force=True`.

`basis_release` preserva la base del trabajo cuando corresponde.

## Source -> Projection

```text
ProjectionTarget =
    SourceKey
    +
    SourceReleaseRef
```

`project(target)`:
- lee release exacta;
- no consulta current durante ejecución;
- conserva provenance exacto.

## Manager root Projection checkpoint

`MANAGER-ROOT-CANONICAL-CUTOVER` permanece:

```text
CLOSED / VERIFIED / CURRENT
```

El cierre no convirtió publication/history legacy textual en release identity.

## Manager exact-source publication checkpoint

Nuevo boundary cerrado en:

```text
moragaga/atlanticus@9342769a626c39d1f7f860f81e051e2ef1300620
```

Contrato:

```text
ExactSourcePublicationWorkflow
    get_source_snapshot() -> SourceSnapshot
    publish_draft_exact(
        payload,
        expected_source_snapshot
    ) -> ExactSourcePublicationResult
```

`ExactSourcePublicationResult.source` es `PublishResult`.

Manager no convierte:
- `SourceReleaseRef`;
- `ConcurrencyToken`;
- `SourceSnapshot`;

a `source_revision: str`.

El protocolo es opt-in y coexiste con workflows legacy.

## Navigation checkpoint

Navigation mantiene:
- canonical Source;
- History Source;
- exact reads;
- Projection Local/Cosmos;
- runtime consumer canónico.

Administración Navigation permanece PLANNED.

Legacy deletion Navigation permanece BLOCKED.

## Users checkpoint

Users Configuration implementa:
- Source codec/service sobre Source Core;
- History y exact reads;
- Source multi-resource Users+Profiles;
- canonical payload `UsersProfilesConfiguration`;
- `UsersProjectionBuilder`;
- exact `SourceProjectionService`;
- Cosmos Projection schema `2` con lectura histórica schema `1`;
- provenance canónico en Projection;
- CAS/idempotencia exact-target.

Admin composition backend implementa:
- `UsersProfilesAdminDraft`;
- `UsersProfilesAdministrationService`;
- exact `SourceSnapshot` como base;
- publication con `ConcurrencyToken` + `basis_release`;
- Profile delete/reassign;
- Managed creation desde Pending.

Manager implementa generic exact-source boundary.

Estado:

```text
USERS-CANONICAL-SOURCE-1          CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2      CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER    CLOSED / VERIFIED / CURRENT
ADMIN-COMPOSITION-BACKEND         CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY     CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-COMPOSITION  IN PROGRESS
```

Todavía no implementado:
- callbacks/layout/browser store canónico;
- Users workflow conectado a `ExactSourcePublicationWorkflow`;
- runtime provenance exact-release;
- legacy deletion;
- resource topology físico canonical Users Projection.

No existe contrato vigente que permita tratar `UsersConfigurationBundle.revision` como `SourceReleaseId`.

## Pendiente

Fuera de Source/Projection Core y boundaries ya cerrados:
- `ADMIN-UI-DRAFT-CUTOVER`;
- `USERS-MANAGER-EXACT-SOURCE-WIRING`;
- retention/cleanup policy;
- provenance exact-release de `users.runtime`;
- migraciones administrativas Navigation;
- browser WORKSPACE/IndexedDB global;
- retiro efectivo de SharePoint/Power Automate por consumidor;
- eliminación legacy sólo después de validar consumidores;
- orchestration multi-capability cuando exista requisito real.
