# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

Corte de implementación:
`moragaga/atlanticus@7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98`.

Parent inmediato:
`moragaga/atlanticus@567e1a12c862b46dfd7f4ec75c3be750c95bbd54`.

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
USERS-PROFILES-DOMAIN-SEPARATION              IN PROGRESS
USERS-PROFILES-ADMIN-COMPOSITION              IN PROGRESS
ADMIN-UI-DRAFT-CUTOVER                        PLANNED / NEXT
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER PLANNED
USERS-RUNTIME-CANONICAL-CUTOVER               PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE        PLANNED
USERS-ADMIN-CANONICAL-MIGRATION               PLANNED
NAV-CONSUMER-MIGRATION-B                      PLANNED
```

## Plataforma genérica

Atlanticus mantiene fronteras separadas para backend jobs, connectivity, operational data, Web capabilities, Source/Projection y scopes/aplicaciones.

`backend/` representa backend jobs y capacidades propias de esos jobs.

La lógica Python server-side cuya responsabilidad es Web pertenece a `web/`.

Connectivity es dual-use y no adquiere ownership funcional de sus consumidores.

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

El camino backend canónico opera directamente sobre `UsersProfilesConfiguration`.

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

La UI/browser store productiva todavía no usa este contrato.

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
- eliminar Profile referenciado requiere `replacement_profile_key`;
- reasignación + eliminación ocurre en una sola transformación;
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

Checkpoint:

```text
moragaga/atlanticus@7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98
```

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

### Límite importante

El composition root productivo ADA todavía registra:

```text
UsersManagerWorkflowAdapter(dependencies.users)
```

Ese adapter:
- usa `UsersConfigurationCatalog`;
- usa `publish_draft(... expected_source_revision: str | None)`;
- sigue siendo legacy.

Por tanto:

```text
USERS-MANAGER-EXACT-SOURCE-COMPOSITION         CLOSED / VERIFIED / CURRENT
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER  PLANNED
```

No afirmar que el Manager productivo de Users ya publica mediante `publish_draft_exact(...)`.

## Web compositions

`web/compositions` no nació con este hito.

`navigation-activity` ya existía y conecta Navigation con el pequeño contrato `ActivityRouteResolver` de Users Activity.

Ownership:
- Users Activity posee tracking de actividad del actor;
- Navigation posee definición de rutas;
- `navigation-activity` adapta una definición Navigation a route keys de Activity;
- la composición no transfiere ownership.

El patrón se reutiliza para `users-manager` porque la integración necesita conocer ambas capabilities sin invertir sus dependencias.

No usar `compositions/` como cajón genérico: sólo cuando dos capabilities independientes necesitan un binding explícito que ninguna debe poseer.

## Legacy administrative configuration

El aggregate histórico `UsersConfigurationCatalog`, `UsersAdministrationService`, `UsersConfigurationBundle`, contracts y callbacks legacy siguen presentes para consumidores no migrados.

Su shape no es el contrato canónico nuevo de Source/Projection ni el payload del nuevo backend admin composition.

La compatibilidad v1 durable es lectura histórica, no shim runtime.

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

## Qualification de los checkpoints recientes

### Draft baseline semantics — `567e1a12...`

Qualification reportada por el usuario:

```text
focused tests        19 passed
Ruff                  GREEN
full Web suite        580 passed, 7 skipped
Python runtime        3.14.7
```

### Users Manager exact-source composition — `7ffebdbb...`

Qualification reportada por el usuario:

```text
uv lock --check                         GREEN
composition focused tests              7 passed
Ruff composition src/tests             GREEN
full Web suite                          587 passed, 7 skipped
git diff --check                        GREEN
```

`atlanticus:main` fue verificado apuntando a `7ffebdbb...`.

No se afirma CI remoto adicional.

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
- delete/reassign atómico;
- creación Managed desde Pending;
- Manager exact-source opt-in boundary;
- adapter/composition exact-source Users↔Manager.

Permanece fuera:
- callbacks/layout/store administrativo productivo canónico;
- registro productivo del exact-source workflow Users en el Manager host;
- runtime canonical cutover;
- exact-release provenance en `users.runtime`;
- eliminación legacy;
- resource topology físico canonical Users Projection;
- configuración física Root;
- contrato final Local/John/Jane.

## Siguiente frontera recomendada

Un único foco:

```text
ADMIN-UI-DRAFT-CUTOVER  PLANNED / NEXT
```

Objetivo: migrar callbacks, layout y browser draft store de Users Configuration al `UsersProfilesAdminDraft` schema 2 / `UsersProfilesConfiguration`, sin mezclar todavía runtime/provenance ni eliminación legacy global.

El productivo exact-source service cutover permanece como incremento separado posterior.
