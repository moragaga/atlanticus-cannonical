# Source Storage — Open Contracts

Estado: **IN PROGRESS — POST USERS MANAGER EXACT LIFECYCLE**

## Core Source — CLOSED

Congelado:

1. `SourceKey`.
2. `SourceReleaseId`.
3. `SourceReleaseRef`.
4. release id separado de content hash.
5. immutable releases.
6. manifest commit point.
7. `basis_release`.
8. `SourceStore`.
9. `ConcurrencyToken`.
10. CAS/current promotion.
11. History con cursor opaco.
12. exact reads.
13. integrity.
14. same-content republish puede crear nueva release.

## Blob — CLOSED

Congelado:

- `StorageClient` compuesto externamente;
- ETag privado al provider;
- `ConcurrencyToken` provider-neutral;
- first publish create-only;
- updates condicionales;
- ACK recovery;
- Local/Blob semantics equivalentes;
- retention/cleanup fuera de `SourceStore`.

## Projection Handoff — CLOSED

Congelado:

- `ProjectionTarget = SourceKey + SourceReleaseRef`;
- `project(target)` no relee current;
- exact release provenance;
- CURRENT/OUTDATED por release identity;
- retry mismo target;
- failure no revierte Source;
- `ProjectionStore.get_active/replace_active`.

## Users canonical Projection — CLOSED

CURRENT payload:

```text
UsersProfilesConfiguration
```

La formulación histórica `ProjectionStore[UsersConfigurationCatalog]` queda SUPERSEDED.

Provider Cosmos conserva:

- exact provenance;
- schema `2` write;
- schema `1` historical read;
- ETag/CAS;
- same-target idempotency.

## Manager exact consumer — CLOSED para Users

Users Manager actual usa:

```text
DraftValidationWorkflow
ExactSourceReaderWorkflow
ExactSourcePublicationWorkflow
ExactSourceHistoryWorkflow
ExactProjectionWorkflow
```

No usa legacy lifecycle service.

History usa:

```text
HistoryPage
SourceReleaseRef
```

Status/Projection usa modelos `projection/core`.

`UsersManagerWorkflowAdapter` está removido.

## OPEN — Users runtime

- canonical runtime cutover;
- exact-release provenance en `users.runtime`;
- definir campos durable exactos antes de consumidores;
- no introducir shim release/string.

## OPEN — non-Users ADA legacy Projection alignment

Full ADA current expone cuatro adapters incompatibles con el contrato Manager vigente:

- Navigation;
- Tools;
- KPI;
- KPI Definitions.

Foco recomendado:

```text
ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT
```

No convertir automáticamente esos dominios a exact-source si no existe requisito. El problema inmediato es Projection target/result.

## OPEN — consumer migrations

- Navigation administrative migration;
- consumers Users legacy fuera del Manager exacto;
- otros domains/providers según ownership real;
- validar consumers antes de borrar legacy.

## OPEN — resource topology

- physical contract canonical Users Projection store;
- environment provisioning/validation;
- composition root físico cuando corresponda.

## OPEN — operation

- retention;
- orphan GC;
- valores productivos de deployment.

## UNVERIFIED

- external runtime assembly de Users exact Projection;
- external runtime assembly de Users Profiles Administration;
- Docker E2E completo;
- full Web suite current checkpoint;
- Python 3.14.7 qualification current checkpoint.
