# Source Storage — Open Contracts

Estado: **IN PROGRESS — POST-MANAGER-ROOT-CUTOVER**

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

## Cerrado en Projection Handoff

Quedan congelados:

1. Projection target ejecutable: `SourceKey + SourceReleaseRef`.
2. `SourceReleaseId` mantiene identidad de publicación.
3. Projection durable identifica explícitamente `source_release_id`.
4. Provenance durable conserva `source_key` y `source_published_at_utc`.
5. Selección del target puede observar Source current.
6. `project(target)` no consulta Source current.
7. Ejecución resuelve exactamente `SourceStore.read_release(target.source_key, target.source_release)`.
8. Source puede avanzar sin invalidar una Projection exacta ya iniciada.
9. Una Projection exitosa de una release anterior queda `OUTDATED` si Source current avanzó.
10. Retry reutiliza el mismo target y no requiere republish.
11. Projection failure no revierte Source.
12. `CURRENT / OUTDATED` compara identidad de release, no content hash.
13. Alignment durable:
    - `NEVER_PROJECTED`;
    - `CURRENT`;
    - `OUTDATED`.
14. Attempt outcome:
    - `SUCCESS`;
    - `FAILED`.
15. `FAILED` no reemplaza el alignment durable.
16. Projection Core expone `get_active` y `replace_active`.
17. Payload/domain serialization pertenece al dominio consumidor.
18. Projection Core no conoce detalles físicos del provider Source.

Evidencia:
- `atlanticus-web-projection==0.1.0`;
- 15 tests Projection GREEN;
- suite Web global 327 passed, 7 skipped;
- Ruff/format Projection GREEN;
- baseline `moragaga/atlanticus@5b383a3ff4dcbb2cc15f55df4819ebf9e61e63b4`.

## Cerrado para Users Configuration / Cosmos

En:

```text
moragaga/atlanticus@139ee93a118e51f66c3d585f00235f212a2475c1
```

queda validado para `CosmosUsersConfigurationProjectionStore`:

1. `ProjectionStore[UsersConfigurationCatalog]`.
2. Builder desde Source exacta mediante `UsersSourceCodec`.
3. First active write create-only.
4. Replace active con ETag/CAS.
5. No blind upsert.
6. Same exact release + same payload = retry idempotente.
7. Same release ID + metadata incompatible = invariant failure.
8. Same exact release + payload incompatible = invariant failure.
9. Conflicto concurrente same-target = éxito idempotente.
10. Conflicto concurrente different-target = error explícito.
11. Target histórico explícito permitido.
12. No ordering inferido por release ID ni `projected_at_utc`.
13. Provenance = `source_key`, `source_release_id`, `source_published_at_utc`, `projected_at_utc`.
14. `UsersConfigurationBundle.revision` no cruza la frontera canónica.

Este cierre es domain/provider-specific y no cierra los contratos equivalentes para otros dominios/providers.

## Cerrado en Manager root Projection

En:

```text
moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
```

quedan congelados para el root productivo de Project:

1. `ConfigurationLifecycleWorkflow.get_current_projection_target() -> ProjectionTarget | None`.
2. `ConfigurationLifecycleWorkflow.project(target: ProjectionTarget)`.
3. `ProjectionExecutionResult.target: ProjectionTarget`.
4. `ManagerProjectionCoordinator.project(...)` no transforma el target ni relee current.
5. La selección current del callback ocurre server-side inmediatamente antes de ejecutar.
6. Browser state no define la identidad ejecutable.
7. Project signal expone identidad exact-release.
8. El botón Project depende de existencia de target exacto.
9. Un target histórico explícito se transporta sin ser sustituido por current.
10. No se introduce shim `SourceReleaseId <-> str` ni segundo coordinator.

La formulación previa de “Manager productive coordinator cutover” queda refinada: los contratos administrativos de publicación, verificación y history que todavía usan revisiones textuales no fueron parte de este cierre.

## Abierto después del Manager root cutover

### Siguiente frontera recomendada — Users runtime exact-release provenance

Estado: **PLANNED / OPEN**.

Pendiente:
- reemplazar `projection_source_revision` como provenance canónico del Managed runtime;
- definir qué campos exact-release persiste `users.runtime` y su invariante de compatibilidad;
- conservar `projected_by` / `projected_at_utc` sólo si siguen perteneciendo al contrato;
- mantener ownership snapshot-level del writer Managed;
- mantener CAS/ETag y semántica Pending→Resolved/retired/re-add ya cerradas;
- no equiparar `UsersConfigurationBundle.revision` con `SourceReleaseId`;
- no introducir shim `SourceReleaseId <-> str`.

El bloqueo “esperar Manager root cutover” queda `SUPERSEDED`: esa precondición ya está satisfecha.

### Manager administrativo

Permanece abierto por separado:
- publicación/verificación/source status legacy basados en revisiones textuales;
- compare;
- conflict workflow completo;
- restore orchestration;
- browser WORKSPACE/IndexedDB.

No reabrir el root Projection exact-target para resolver estos frentes.

### Consumer migrations

Pendiente:
- migración administrativa Navigation;
- migración administrativa Users;
- auditar composition roots reales antes de fijar nombres de adapters;
- no asumir que existen `NavigationManagerWorkflowAdapter` o `UsersManagerWorkflowAdapter` en `main`;
- validar consumidores antes de borrar legacy.

### Projection providers y orchestration

Pendiente:
- implementar/adaptar providers de otros dominios según ownership real;
- validar atomicidad/durabilidad de `replace_active` para cada provider nuevo;
- congelar idempotencia provider/domain-level para cada provider nuevo;
- projection planner/orchestration multi-capability;
- derived resolutions cuando existan dependencias reales.

Users/Cosmos ya satisface estos contratos para su provider concreto.

### Resource topology de canonical Users Projection

Pendiente:
- congelar resource topology/provisioning físico del canonical Projection store si el deployment lo requiere;
- validar composition root productivo cuando corresponda.

### Migración legacy

Pendiente:
- identificar rutas exactas a reemplazar/eliminar;
- conservar contracts legacy mientras exista consumer productivo;
- retirar SharePoint/Power Automate Source sólo dentro del incremento de migración correspondiente; el gate de paridad/recovery ya está satisfecho.

### Operación

Pendiente:
- retention;
- GC/orphan cleanup;
- valores productivos concretos de deployment.

Estas políticas permanecen fuera de `SourceStore` y de Projection Core.
