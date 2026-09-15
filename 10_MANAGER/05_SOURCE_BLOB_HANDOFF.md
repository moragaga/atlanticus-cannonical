# Manager — Source Blob Handoff

Estado: **SOURCE IMPLEMENTED / ROOT PROJECTION CUTOVER CLOSED / EXACT-SOURCE BOUNDARY CLOSED / USERS EXACT-SOURCE COMPOSITION CLOSED / CONSUMER MIGRATION IN PROGRESS**

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
Users Manager exact-source composition adapter
```

Source Core cubre release identity, release granularity, manifest, `SourceStore`, concurrency/current, History, exact reads e integrity verification.

Blob cubre manifest como commit point, create-only first publish, conditional write, `ConcurrencyToken` opaco, releases inmutables, recovery e integridad.

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

`basis_release` preserva la base/provenance del trabajo según el contrato de publication.

## Source -> Projection

```text
ProjectionTarget =
    SourceKey
    +
    SourceReleaseRef
```

`project(target)` lee release exacta, no consulta current durante ejecución y conserva provenance exacto.

## Manager checkpoints

Root Projection:

```text
MANAGER-ROOT-CANONICAL-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Generic exact-source:

```text
MANAGER-EXACT-SOURCE-BOUNDARY
CLOSED / VERIFIED / CURRENT
moragaga/atlanticus@9342769a626c39d1f7f860f81e051e2ef1300620
```

Users exact-source composition:

```text
USERS-MANAGER-EXACT-SOURCE-COMPOSITION
CLOSED / VERIFIED / CURRENT
moragaga/atlanticus@7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98
```

Manager no convierte `SourceReleaseRef`, `ConcurrencyToken` ni `SourceSnapshot` a `source_revision: str`.

## Users checkpoint

Users Configuration implementa:
- Source codec/service;
- History y exact reads;
- Source multi-resource Users+Profiles;
- canonical payload `UsersProfilesConfiguration`;
- exact Source→Projection service;
- Cosmos Projection schema `2` con lectura histórica schema `1`;
- provenance canónico en Projection;
- CAS/idempotencia exact-target.

Admin backend implementa:
- `UsersProfilesAdminDraft` schema `2`;
- `base_payload_revision`;
- clean/dirty/rebase local;
- `UsersProfilesAdministrationService`;
- exact `SourceSnapshot`;
- publication con `ConcurrencyToken` + `basis_release`;
- Profile delete/reassign;
- Managed creation desde Pending.

Users↔Manager composition implementa:
- `UsersManagerExactSourceWorkflow`;
- strict canonical payload parse;
- actor provider;
- exact publication delegation;
- typed `ExactSourcePublicationResult`;
- audit timestamp desde release publicada.

Estado:

```text
USERS-CANONICAL-SOURCE-1                      CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2                  CLOSED / VERIFIED / CURRENT
ADMIN-COMPOSITION-BACKEND                     CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY                 CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-DRAFT-BASELINE-SEMANTICS CLOSED / VERIFIED / CURRENT
USERS-MANAGER-EXACT-SOURCE-COMPOSITION        CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-COMPOSITION              IN PROGRESS
```

## Consumer migration todavía pendiente

No implementado productivamente:
- callbacks/layout/browser store canónico;
- registro del exact-source Users workflow en el host ADA;
- publication action productiva mediante `publish_draft_exact(...)`;
- runtime provenance exact-release;
- legacy deletion;
- resource topology físico canonical Users Projection.

El host ADA todavía usa `UsersManagerWorkflowAdapter` legacy.

No existe contrato vigente que permita tratar `UsersConfigurationBundle.revision` como `SourceReleaseId`.

## Pendiente

Fuera de Source/Projection Core y boundaries cerrados:
- `ADMIN-UI-DRAFT-CUTOVER`;
- `USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER`;
- retention/cleanup policy;
- provenance exact-release de `users.runtime`;
- migraciones administrativas Navigation;
- browser WORKSPACE/IndexedDB global;
- retiro efectivo de SharePoint/Power Automate por consumidor;
- eliminación legacy sólo después de validar consumidores;
- orchestration multi-capability cuando exista requisito real.
