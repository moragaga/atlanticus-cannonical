# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

Corte de implementación:
`moragaga/atlanticus@d23bff025ab899367a8da1178dde5ab50806fe47`.

Parent inmediato:
`moragaga/atlanticus@7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98`.

## Resumen de estado

```text
WEB-STORAGE-TOPOLOGY                          CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY                        CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE               CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER                  CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY             CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1                      CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2                  CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER                CLOSED / VERIFIED / CURRENT
PROFILES-DOMAIN-EXTRACTION                    CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS                   CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION                     CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT                CLOSED / VERIFIED / CURRENT
ADMIN-COMPOSITION-BACKEND                     CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY                 CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-DRAFT-BASELINE-SEMANTICS CLOSED / VERIFIED / CURRENT
USERS-MANAGER-EXACT-SOURCE-COMPOSITION        CLOSED / VERIFIED / CURRENT
ADMIN-UI-DRAFT-CUTOVER                        CLOSED / VERIFIED / CURRENT
USERS-PROFILES-DOMAIN-SEPARATION              IN PROGRESS
USERS-PROFILES-ADMIN-COMPOSITION              IN PROGRESS
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER PLANNED / NEXT CANDIDATE
USERS-RUNTIME-CANONICAL-CUTOVER               PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE        PLANNED
USERS-ADMIN-CANONICAL-MIGRATION               PLANNED
NAV-CONSUMER-MIGRATION-B                      PLANNED
DOMAIN-LEGACY-DELETION                        BLOCKED
```

## Plataforma genérica

Atlanticus mantiene fronteras separadas para backend jobs, connectivity, operational data, Web capabilities, Source/Projection y scopes/aplicaciones.

`backend/` representa backend jobs y capacidades propias de esos jobs. La lógica Python server-side cuya responsabilidad es Web pertenece a `web/`. Connectivity es dual-use y no adquiere ownership funcional de sus consumidores.

## Storage Topology y Users durable

`web/capabilities/storage/topology` define contratos provider-neutral para resource topology.

Users declara un único recurso durable runtime confirmado:

```text
logical_id              users.runtime
owner                   users
provider                cosmos
default_physical_name   users-runtime
partition_key_path      /id
default_ttl_seconds     None
```

Invariantes CURRENT:
- Pending y Managed comparten `users.runtime`;
- `id == partition key == user_id`;
- `user_id = build_user_key(issuer, subject_id)`;
- no TTL automático para datos durables Users;
- connection binding lo provee composición;
- no se crean `users.pending`, `users.managed`, `users.projection` ni `profiles.runtime` por defecto.

`CosmosUsersRuntimeStore` implementa `UsersRuntimeStore` + `PendingUsersReader`.

`observe()` es create-only + conflict reread; no usa blind upsert.

El writer administrativo Managed permanece separado del runtime reader/observer y usa CAS/ETag.

Managed removal conserva el documento como Resolved, disabled y `managed_state=retired`; re-add restaura `managed_state=present`.

## Source / Projection exact-release

Contratos congelados:
- release identity != content hash;
- dos releases pueden compartir content hash;
- Source current lo decide Source, nunca Cosmos;
- Projection target ejecutable = `SourceKey + SourceReleaseRef`;
- `project(target)` usa la release exacta y no relee current;
- `CURRENT / OUTDATED` compara identidad de release;
- retry conserva el mismo target;
- no introducir shim `SourceReleaseId <-> str`.

Users dispone de Source canónico y Projection canónica exact-release.

Una exact Source release de Users Configuration contiene dos resources contractualmente separados:

```text
users/configuration.json.gz
profiles/configuration.json.gz
```

La release sigue siendo única y atómica desde la perspectiva de Source; no existe Source independiente de Profiles ni segundo coordinator.

Escritura nueva:
- Users source document schema `2`;
- Profiles source resource schema `1`;
- `users/configuration.json.gz` contiene `UsersConfiguration` + `published_by`;
- `profiles/configuration.json.gz` contiene `ProfilesConfiguration`;
- Guest durable fields no forman parte del contrato canónico nuevo.

Lectura histórica:
- Source schema Users `1` continúa soportado;
- se normaliza `UsersConfigurationCatalog` histórico hacia contratos separados;
- Administrator se materializa como `ProfileDefinition(key="administrator", label="Administrador", ...)`;
- campos Guest históricos no crean Profile funcional.

Projection canónica vigente:

```text
ProjectionRecord[UsersProfilesConfiguration]
```

El Cosmos canonical Projection store escribe schema `2`, puede leer schema `1`, conserva exact-release provenance y CAS/ETag.

El provenance legacy dentro de `users.runtime` todavía usa `projection_source_revision`; su migración exact-release sigue PLANNED.

## Profiles Domain

`ProfilesConfiguration` vive en Profiles y conserva exclusivamente Profiles funcionales explícitos.

Ownership CURRENT:
- Profiles no depende de Users;
- Users puede depender de Profiles;
- `ProfilesConfiguration` no introduce Source/Projection propia de Profiles;
- no existe `profiles.runtime` por inferencia.

`ProfileCatalog` permanece semánticamente puro:
- catálogo vacío significa vacío;
- sólo contiene `ProfileDefinition` explícitos;
- no fabrica Local, Administrator ni Guest;
- duplicate normalized keys fallan;
- `require()` normaliza la key.

## Users canonical configuration

`UsersConfiguration` posee exclusivamente Managed Users y valida:
- `user_id` único;
- email no nulo único;
- identidad `(issuer, subject_id)` única.

`UsersProfilesConfiguration` posee la validación cross-contract:
- exige Profile `administrator`;
- prohíbe Profiles funcionales `guest` y `local`;
- cada Managed User, enabled o disabled, debe referenciar un Profile existente mediante `profile_key`.

## Admin composition backend

El backend canónico opera directamente sobre `UsersProfilesConfiguration`.

Contratos CURRENT:
- `UsersProfilesAdminState`;
- `UsersProfilesAdminDraft`;
- `UsersProfilesAdministrationService`;
- operaciones puras de edición sobre `UsersProfilesConfiguration`.

### Draft canónico — schema 2

`UsersProfilesAdminDraft` contiene:
- `owner_subject_id`;
- `configuration: UsersProfilesConfiguration`;
- `source_snapshot: SourceSnapshot`;
- `revision`;
- `base_payload_revision`;
- `saved_at_utc`.

Documento:
- `document_type = "atlanticus_users_profiles_admin_draft"`;
- `schema_version = 2`;
- payload = `UsersProfilesConfiguration`;
- serializa el `SourceSnapshot` exacto, incluido current release y `ConcurrencyToken` cuando existen.

Semántica local congelada:
- `revision` = SHA-256 del JSON canónico del payload actual;
- `base_payload_revision` identifica el payload que constituye la BASE local;
- draft recién creado: `revision == base_payload_revision`;
- `has_local_changes` depende únicamente de esas dos revisiones locales;
- `with_configuration(...)` cambia `revision` y preserva BASE + exact `SourceSnapshot`;
- `rebase(new_source_snapshot)` conserva payload, adopta el nuevo exact `SourceSnapshot` y hace `base_payload_revision = revision`;
- revisión local no es `SourceReleaseId`, `content_hash` ni `ConcurrencyToken`;
- parser schema 2 no acepta schema 1 ni shape legacy.

### Operaciones administrativas canónicas

Administrator:
- Profile explícito;
- no puede eliminarse;
- key estable;
- operación dedicada actual cambia colores y preserva label/key.

Profiles funcionales:
- creación deriva key desde label;
- edición preserva key;
- Administrator no se edita mediante operación genérica;
- eliminar Profile no referenciado es válido;
- eliminar Profile referenciado requiere `replacement_profile_key` en el backend;
- reasignación + eliminación ocurre en una sola transformación backend;
- la regla incluye Users disabled.

Managed Users:
- alta administrativa canónica parte de `PendingUserRecord`;
- conserva `user_id`, `issuer` y `subject_id` del Pending;
- no existe upsert genérico que invente una identidad Managed;
- actualización exige User existente;
- `(issuer, subject_id)` no puede cambiarse.

Pending:
- `list_pending(configuration)` excluye identidades ya configuradas.

### Source exacto desde Users admin

`UsersProfilesAdministrationService`:
- carga current mediante `UsersSourceService`;
- si carga una release, revalida que el `SourceSnapshot` no haya cambiado;
- publica sólo si el snapshot esperado coincide con current;
- valida `SourceKey`;
- exige actor no vacío;
- usa `expected_source_snapshot.concurrency_token` como precondición;
- para publication normal usa la `release_ref` del snapshot esperado como `basis_release`;
- delega en `UsersSourceService.publish_configuration(...)`.

## Admin UI draft cutover

Checkpoint:

```text
moragaga/atlanticus@d23bff025ab899367a8da1178dde5ab50806fe47
```

Estado:

```text
ADMIN-UI-DRAFT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

El camino Web activo de Users Configuration usa:

```text
canonical_layout.py
canonical_callbacks.py
UsersAdminWebContext.administration: UsersProfilesAdministrationService
UsersProfilesConfiguration
UsersProfilesAdminDraft schema 2
```

Invariantes de UI CURRENT:
- `CATALOG_STORE_ID` contiene `UsersProfilesConfiguration.to_document()`;
- `DRAFT_BASIS_STORE_ID` conserva el draft/basis schema 2 en memoria;
- el editor revision usa `build_users_profiles_admin_revision(...)`;
- guardar draft usa `basis.with_configuration(configuration)` y persiste el mismo documento schema 2 en draft/saved/basis stores;
- guardar draft no publica Source;
- schema 1/browser draft incompatible no se convierte: se descarta y se crea una base limpia desde Source current, con aviso visible;
- Administrator usa la operación canónica dedicada;
- Profile create/edit usa operaciones canónicas;
- un Profile referenciado no se elimina desde la UI actual; la UX de replacement explícito permanece OPEN;
- Managed edit preserva identidad;
- alta nueva parte de Pending y revalida que la identidad siga pendiente;
- import de archivo legacy se decodifica explícitamente y se transforma al contrato canónico dentro de la BASE actual; esto es compatibilidad de import, no migración de browser draft;
- el active UI no reconstruye `UsersConfigurationCatalog` para editar/guardar.

`canonical_layout.py` usa `dcc.Store(storage_type="memory")`; este hito no implementa IndexedDB global de Manager.

Los archivos legacy Web pueden seguir físicamente presentes hasta el cleanup global, pero ya no son el path exportado/registrado activo para el editor Users.

## Manager exact-source boundary

Manager conserva un contrato opt-in:

```text
ExactSourcePublicationWorkflow
    get_source_snapshot() -> SourceSnapshot
    publish_draft_exact(
        payload,
        expected_source_snapshot
    ) -> ExactSourcePublicationResult
```

`ExactSourcePublicationResult` conserva `PublishResult` tipado de Source, audit y summary.

`ManagerProjectionCoordinator`:
- aplica autorización;
- compara el `SourceSnapshot` completo antes de publicar;
- no reduce release/token a string;
- si el workflow falla y Source cambió, convierte el caso en `ManagerSourceConflictError`;
- resuelve exact-source por separado del workflow legacy.

La garantía CAS final sigue perteneciendo a Source/workflow.

## Users ↔ Manager exact-source composition

Implementado en:

```text
web/compositions/users-manager
```

`UsersManagerExactSourceWorkflow`:
- satisface estructuralmente `ExactSourcePublicationWorkflow`;
- `get_source_snapshot()` delega a `UsersProfilesAdministrationService`;
- `publish_draft_exact(...)` parsea estrictamente `UsersProfilesConfiguration`;
- obtiene actor mediante `UsersAuditActorProvider`;
- delega publication exacta al backend administrativo Users;
- retorna `ExactSourcePublicationResult`;
- toma `audit.occurred_at` de la release confirmada por Source;
- no persiste draft;
- no hace `rebase`;
- no proyecta;
- no conoce Dash/IndexedDB;
- no registra por sí mismo servicios del host.

Dirección de dependencias CURRENT:

```text
Manager                 Users Configuration
   ↑                           ↑
   └──── compositions/users-manager ────┘
```

Manager no depende de Users y Users Configuration no depende de Manager.

## Productive host boundary

El cutover UI no equivale al cutover productivo de Manager.

El host ADA conserva el registro legacy para Users:

```text
UsersManagerWorkflowAdapter(dependencies.users)
```

Ese adapter sigue asociado al contrato legacy de `UsersConfigurationCatalog` / `expected_source_revision: str | None`.

El composition root de ADA ahora exige además:

```text
users_profiles_administration: UsersProfilesAdministrationService
```

y construye `UsersAdminWebContext(administration=...)` para el editor canónico.

No se identificó en `atlanticus` un constructor productivo de `ConfigurationManagerDependencies`; la inyección física externa de `users_profiles_administration` permanece UNVERIFIED.

Por tanto:

```text
USERS-MANAGER-EXACT-SOURCE-COMPOSITION         CLOSED / VERIFIED / CURRENT
ADMIN-UI-DRAFT-CUTOVER                         CLOSED / VERIFIED / CURRENT
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER  PLANNED
```

No afirmar que el Manager productivo de Users ya publica mediante `publish_draft_exact(...)`.

## Legacy administrative configuration

`UsersConfigurationCatalog`, `UsersAdministrationService`, `UsersConfigurationBundle` y contratos legacy siguen presentes para consumidores no migrados.

Refinamiento CURRENT:
- ya no son el contrato del editor Users activo;
- `UsersManagerWorkflowAdapter` productivo sigue legacy;
- `build_users_history_preview` sigue ligado al camino legacy;
- el import de archivo legacy conserva compatibilidad explícita de lectura y transforma al payload canónico;
- la compatibilidad durable histórica no justifica un adapter de authoring canónico→legacy.

## Web compositions

`navigation-activity` ya existía y conecta Navigation con `ActivityRouteResolver` de Users Activity.

`users-manager` reutiliza el patrón sólo porque la integración necesita conocer Manager y Users Configuration sin invertir dependencias.

No usar `compositions/` como cajón genérico.

## Pending / Guest

Pending pertenece a Users, no a Profiles.

`EffectiveUser.profile` puede ser `None`.

Guest no es Profile runtime ni Profile funcional configurable en el contrato canónico.

## Root bootstrap

Root pertenece a Identity/bootstrap, no a Profiles ni al flujo normal Managed Users.

La fuente física de `BootstrapRootPolicy` y el mapping exacto Entra permanecen UNVERIFIED / OPEN.

## Service composition

Users no publica Profiles como servicio propio.

Contrato CURRENT:

```text
create_users_module(runtime)
→ registra únicamente USERS_RUNTIME_SERVICE_KEY
```

`PROFILE_CATALOG_SERVICE_KEY = "atlanticus.web.users.profiles"` permanece eliminado.

## Local / John / Jane

Local, John y Jane quedan fuera de Profiles semánticos.

Su contrato runtime/ownership final permanece OPEN.

## Qualification del cierre `d23bff...`

Qualification reportada/ejecutada en el workspace real:

```text
Users focal pytest                         15 passed
Users Ruff                                 GREEN
ADA composition + mirror scoped pytest     5 passed
ADA scoped Ruff                            GREEN
full Web suite                             587 passed, 7 skipped
full Web Ruff                              GREEN
web uv lock --check                        GREEN (75 packages)
git diff --check                           GREEN
Python runtime usado                       3.14.2
```

`atlanticus:main` fue verificado read-only apuntando exactamente a `d23bff...`.

No se afirma CI remoto adicional.

### Qualification ADA host — conflicto de packaging/contratos

VERIFIED:
- `ada-configuration-manager` lock resuelve `atlanticus-web-manager 0.3.14` y `atlanticus-web-users-configuration 0.1.6`;
- los sources actuales inspeccionados son Manager `0.3.15` y Users Configuration `0.1.9`;
- el entorno frozen falló al importar `atlanticus.web.projection` por el drift Manager lock/source;
- existía un editable residual local `ada-web-tool-configuration-editor 0.2.0` que interceptaba `ada.web.configuration`; se eliminó del `.venv` local y el import correcto quedó restaurado;
- con overlay efímero de Manager/Users actuales, `tests/test_composition.py` pasó `4/4`;
- después de actualizar mirrors del incremento, `test_commented_mirror.py + test_composition.py` pasó `5/5`.

La suite ADA completa con overlay actual produjo antes del fix de mirror:

```text
54 passed
6 failed
```

Un failure era el mirror de este incremento y quedó corregido/verificado en la suite scoped posterior.

Los otros cinco failures exponen adapters ADA legacy que no satisfacen el contrato vigente de Manager `0.3.15` (`ProjectionTarget` / `ProjectionExecutionResult.target`). La suite completa no se volvió a ejecutar después del fix de mirror, por lo que el conteo final global ADA permanece UNVERIFIED.

No corregir esos adapters dentro de `ADMIN-UI-DRAFT-CUTOVER`.

## Python baseline

El Project mantiene decidido Python `3.14.7` + `python:3.14.7-slim-trixie`.

En este cierre:
- `uv python find 3.14.7` no encontró intérprete local;
- runtime disponible/usado fue Python `3.14.2`;
- varios `pyproject.toml` actuales todavía declaran `==3.14.2`.

Por tanto, qualification de este checkpoint bajo Python `3.14.7` permanece BLOCKED / UNVERIFIED y la migración global no forma parte de este hito.

## Frontera Users / Profiles completa

Estado:

```text
USERS-PROFILES-DOMAIN-SEPARATION  IN PROGRESS
```

Cerrado:
- ownership durable contractual Users vs Profiles;
- representación canónica Administrator;
- Guest fuera del nuevo contrato funcional;
- cross-contract orphan validation;
- Source único con dos resources;
- Projection payload compuesto;
- read-v1/write-v2;
- backend admin composition sobre `UsersProfilesConfiguration`;
- draft backend con exact `SourceSnapshot`;
- baseline local `revision/base_payload_revision`;
- delete/reassign backend atómico;
- creación Managed desde Pending;
- Manager exact-source opt-in boundary;
- adapter/composition exact-source Users↔Manager;
- callbacks/layout/store administrativo activo sobre schema 2 / `UsersProfilesConfiguration`.

Permanece fuera:
- productive exact-source service registration/publication en Manager;
- UX de replacement para delete de Profile referenciado;
- wiring físico externo de `users_profiles_administration`;
- runtime canonical cutover;
- exact-release provenance en `users.runtime`;
- eliminación legacy;
- resource topology físico canonical Users Projection;
- configuración física Root;
- contrato final Local/John/Jane;
- migración Python 3.14.7.

## Siguiente frontera recomendada

Un único foco de debate/diseño:

```text
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER
PLANNED / NEXT CANDIDATE
```

Antes de implementar, verificar dentro de ese foco la compatibilidad real del host ADA con Manager actual, el registro de servicios, el consumo del draft schema 2 y la inyección productiva de `UsersProfilesAdministrationService`.

No mezclar runtime/provenance, Python migration, Root physical config, Navigation migration ni legacy cleanup global.
