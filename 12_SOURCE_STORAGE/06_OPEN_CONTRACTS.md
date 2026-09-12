# Source Storage — Open Contracts

Estado: **IN PROGRESS**

## Cerrado en SOURCE-1A.1

Quedan congelados para Core + Local:

1. Identidad lógica de Source: `SourceKey`.
2. Identidad de publicación: `SourceReleaseId`.
3. Referencia resoluble: `SourceReleaseRef`.
4. Separación `release_id` vs `content_hash`.
5. Functional manifest.
6. `manifest.json` como único commit point.
7. Release metadata e inventory.
8. `previous_published_release`.
9. `basis_release`.
10. `SourceStore` operations/results/errors.
11. `ConcurrencyToken` opaco.
12. Conditional promotion/CAS semantics.
13. First-publish concurrency.
14. History por predecessor chain.
15. History pagination con cursor opaco.
16. Integridad.
17. Local physical layout.
18. Local durable recovery.
19. Orphans fuera de History.
20. Same-content republish permitido como nueva release.

## Abierto para SOURCE-1A.2 — Blob

Antes de cerrar Blob deben resolverse con evidencia:

1. Qué API exacta de `connectivity/storage` reutiliza Source Blob.
2. Si `connectivity/storage` ya permite:
   - lectura de bytes/metadata necesaria;
   - create-if-absent;
   - conditional write con versión observada;
   - error classification suficiente.
3. Mapping interno ETag → `ConcurrencyToken`.
4. Semántica exacta de create-only para first publish.
5. Recovery ante timeout/ACK ambiguo.
6. Physical layout Blob compatible con `SourceReleaseRef`.
7. Container/root configuration.
8. Storage connection naming:
   - conexión existente;
   - o conexión nombrada `configuration_source`.
9. Requisito o no de HNS.
10. Lifecycle/cleanup de orphans.
11. Retention.

No inferir estos detalles desde Azure ni desde SharePoint legacy.

## Abierto después de Blob

### Projection

Pendiente:
- `source_release_id` durable;
- proyección de una release exacta;
- retry de Projection sin republish;
- estados `NEVER_PROJECTED / CURRENT / OUTDATED / FAILED`.

### Manager

Pendiente:
- BASE/SOURCE/WORKSPACE/PROJECTION;
- compare;
- conflict workflow;
- restore orchestration.

Compare no se añade automáticamente a `SourceStore`; pertenece a la capa que interprete contenido/dominio salvo que aparezca una necesidad genérica demostrada.

### Migración legacy

Pendiente:
- migrar Navigation/Tool Configuration a Source;
- identificar rutas exactas a reemplazar/eliminar;
- conservar Projection local/Cosmos según ownership;
- retirar SharePoint/Power Automate Source sólo después de paridad/recovery.
