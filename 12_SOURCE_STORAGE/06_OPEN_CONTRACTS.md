# Source Storage — Open Contracts

Estado: **IN PROGRESS — POST-BLOB**

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

## Cerrado en SOURCE-1A.2 — Blob

Resuelto con implementación y evidencia:

1. Source Blob reutiliza `StorageClient`.
2. APIs técnicas usadas:
   - `get_properties` para ETag;
   - `download` para bytes;
   - `upload(overwrite=False)` para create-only;
   - `upload_if_match` para conditional write.
3. ETag no se mapea a `ConcurrencyToken`:
   - ETag permanece privado y protege el write remoto;
   - `ConcurrencyToken` se deriva de los bytes exactos del manifest y sigue siendo provider-neutral.
4. First publish promueve `manifest.json` con create-only.
5. ACK ambiguo se recupera releyendo current:
   - candidate current → success;
   - expected current unchanged → not promoted / unavailable;
   - different current → concurrency conflict;
   - no blind republish.
6. Layout Blob conserva la semántica Local:
   - `sources/{encoded_source}/manifest.json`;
   - `history/year=YYYY/month=MM/day=DD/{encoded_release}/release.json`;
   - `resources/{logical_path}`.
7. `BlobSourceSettings` usa:
   - `container_name` requerido;
   - `root_prefix` opcional;
   - el container es preprovisionado;
   - valores productivos concretos siguen siendo deployment-specific.
8. Source recibe un `StorageClient` ya compuesto:
   - no inventa connection name;
   - named connections pertenecen a composition/configuration.
9. HNS no es requisito del provider.
10. Cleanup de orphans queda fuera de `SourceStore`.
11. Retention queda fuera de `SourceStore` y permanece como política posterior.

Evidencia:
- tests deterministas Blob GREEN;
- regresión Web GREEN;
- 7 pruebas Azurite GREEN;
- first-publish conflict real observado como 409;
- stale conditional update real observado como 412;
- recovery de ACK ambiguo validado después de writes reales.

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
- retirar SharePoint/Power Automate Source sólo dentro del incremento de migración correspondiente; el gate de paridad/recovery ya está satisfecho.
