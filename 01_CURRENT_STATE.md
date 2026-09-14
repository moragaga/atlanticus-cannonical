# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

Corte de implementación:
`moragaga/atlanticus@9342769a626c39d1f7f860f81e051e2ef1300620`.

Parent inmediato:
`moragaga/atlanticus@05d6cbb5b81b762f7fc06fc96b7959bfb835a7e3`.

## Resumen de estado

```text
WEB-STORAGE-TOPOLOGY                       CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY                     CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE            CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER               CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY          CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1                   CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2               CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER             CLOSED / VERIFIED / CURRENT
PROFILES-DOMAIN-EXTRACTION                 CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS                CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION                  CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT             CLOSED / VERIFIED / CURRENT
USERS-PROFILES-DOMAIN-SEPARATION           IN PROGRESS
USERS-PROFILES-ADMIN-COMPOSITION           IN PROGRESS
ADMIN-COMPOSITION-BACKEND                  CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY              CLOSED / VERIFIED / CURRENT
ADMIN-UI-DRAFT-CUTOVER                     PLANNED / NEXT
USERS-MANAGER-EXACT-SOURCE-WIRING          PLANNED
USERS-RUNTIME-CANONICAL-CUTOVER            PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE     PLANNED
USERS-ADMIN-CANONICAL-MIGRATION            PLANNED
NAV-CONSUMER-MIGRATION-B                   PLANNED
```

## Plataforma genérica

Atlanticus mantiene fronteras separadas para:
- backend jobs;
- connectivity;
- operational data;
- Web capabilities;
- Source/Projection;
- aplicaciones/scopes.

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

Source Core, Local Source, Blob Source y Projection exact-release están implementados y validados.

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
- Guest durable fields no forman parte del nuevo contrato canónico.

Lectura histórica:
- Source schema Users `1` continúa soportado;
- se normaliza `UsersConfigurationCatalog` histórico hacia los contratos separados;
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

En `9342769a...` existe un nuevo camino backend canónico de administración basado directamente en `UsersProfilesConfiguration`.

No crea un nuevo aggregate mixto de ownership.

Contratos CURRENT implementados:
- `UsersProfilesAdminState`;
- `UsersProfilesAdminDraft`;
- `UsersProfilesAdministrationService`;
- operaciones puras de edición sobre `UsersProfilesConfiguration`.

### Draft canónico

`UsersProfilesAdminDraft` contiene:
- `owner_subject_id`;
- `configuration: UsersProfilesConfiguration`;
- `source_snapshot: SourceSnapshot`;
- `revision`;
- `saved_at_utc`.

Documento:
- `document_type = "atlanticus_users_profiles_admin_draft"`;
- `schema_version = 1`;
- payload = `UsersProfilesConfiguration`;
- serializa el `SourceSnapshot` exacto, incluido current release y `ConcurrencyToken` cuando existen.

`revision` es SHA-256 de JSON canónico de `UsersProfilesConfiguration`.

Es identidad local del draft; no es `SourceReleaseId`, no es `content_hash` y no debe reinterpretarse como release identity.

El parser del nuevo draft no acepta el shape legacy administrativo.

La UI/browser store productiva todavía no usa este contrato; su cutover es el siguiente foco.

### Operaciones administrativas canónicas

Administrator:
- es Profile explícito;
- no puede eliminarse;
- su key permanece estable;
- la operación dedicada actual cambia colores y preserva label/key.

Profiles funcionales:
- creación deriva key desde label;
- edición preserva key;
- Administrator no se edita mediante la operación genérica;
- eliminar Profile no referenciado es válido;
- eliminar Profile referenciado requiere `replacement_profile_key`;
- la operación reasigna todos los Users referenciados y luego elimina el Profile en una sola transformación;
- la regla incluye Users disabled.

Managed Users:
- alta administrativa canónica parte de `PendingUserRecord`;
- conserva `user_id`, `issuer` y `subject_id` del Pending;
- no existe un upsert genérico que invente una identidad Managed;
- actualización exige que el User exista;
- `(issuer, subject_id)` no puede cambiarse.

Pending:
- `list_pending(configuration)` excluye identidades ya configuradas.

### Source exacto desde Users admin

`UsersProfilesAdministrationService`:
- carga current mediante `UsersSourceService`;
- cuando carga una release revalida que el `SourceSnapshot` no haya cambiado;
- publica sólo si el snapshot esperado coincide con current;
- valida `SourceKey`;
- exige actor no vacío;
- usa `expected_source_snapshot.concurrency_token` como precondición;
- usa la `release_ref` de la base como `basis_release`;
- delega en `UsersSourceService.publish_configuration(...)`.

Esto cierra el contrato backend de composición/publicación exacta, pero no migra todavía callbacks/layout/browser draft store.

## Manager exact-source boundary

Manager incorpora un contrato opt-in:

```text
ExactSourcePublicationWorkflow
    get_source_snapshot() -> SourceSnapshot
    publish_draft_exact(
        payload,
        expected_source_snapshot
    ) -> ExactSourcePublicationResult
```

`ExactSourcePublicationResult` conserva `PublishResult` tipado de Source, audit y summary.

`ManagerProjectionCoordinator` agrega:
- `get_exact_source_snapshot(...)`;
- `publish_draft_exact(...)`.

El coordinator:
- aplica autorización;
- compara el `SourceSnapshot` completo antes de publicar;
- no reduce release/token a string;
- si el workflow falla y Source cambió, convierte el caso en `ManagerSourceConflictError`;
- resuelve el protocolo exact-source por separado del workflow legacy.

Este contrato no obliga a módulos legacy a migrar.

Los campos textuales legacy de publication/verification/history continúan existiendo y no se reinterpretan como release identity.

No existe todavía wiring productivo de Users hacia `ExactSourcePublicationWorkflow`.

## Legacy administrative configuration

El aggregate histórico `UsersConfigurationCatalog`, `UsersAdministrationService`, `UsersConfigurationBundle`, contracts y callbacks legacy siguen presentes para consumidores no migrados.

Su shape histórico puede contener:

```text
administrator_background_color
administrator_text_color
guest_background_color
guest_text_color
profiles
users
```

Ese shape no es el contrato canónico nuevo de Source/Projection ni el payload del nuevo backend admin composition.

La compatibilidad v1 durable es lectura histórica, no un shim runtime.

## Pending / Guest

Pending pertenece a Users, no a Profiles.

`EffectiveUser.profile` puede ser `None`.

Para `pending=True`:
- `enabled=True`;
- `profile is None`;
- `is_local=False`;
- no se aceptan overrides de avatar;
- colores estáticos actuales: fondo `#FF5722`, texto `#FFFFFF`.

Guest no es Profile runtime ni Profile funcional configurable en el contrato canónico.

## Administrator

Administrator es un Profile funcional normal y explícito en `ProfilesConfiguration`.

`UsersProfilesConfiguration` exige su presencia.

El aggregate legacy puede continuar sintetizándolo mientras el camino legacy siga vigente.

## Root bootstrap

Root pertenece a Identity/bootstrap, no a Profiles ni al flujo normal Managed Users.

Implementado:
- `BootstrapRootPolicy`;
- `BootstrapRootAccessResolver`;
- `AccessDecision.bootstrap_root`;
- `AccessSnapshot.bootstrap_root`.

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

## Qualification del checkpoint `9342769a...`

Qualification ejecutada por el usuario sobre el workspace real después de integrar el incremento:

```text
focused new tests                    19 passed
Users Configuration package tests   GREEN
Manager package tests               GREEN
commented mirror contract           GREEN
productive/commented compile        GREEN
changed-file Ruff lint              GREEN
changed-file Ruff format check      GREEN
full Web suite                       571 passed, 7 skipped
git diff --check                     GREEN
changed implementation files         12 expected files
```

Notas:
- el primer gate Ruff detectó tres imports ordenables y formato en `admin_composition.py`; fueron corregidos antes de la qualification final;
- no se afirma un `ruff check .` global nuevo para este checkpoint;
- no se afirma CI remoto adicional.

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
- delete/reassign atómico;
- creación Managed desde Pending;
- Manager exact-source opt-in boundary.

Permanece fuera:
- callbacks/layout/store administrativo productivo;
- wiring Users ↔ Manager exact-source;
- runtime canonical cutover;
- exact-release provenance en `users.runtime`;
- eliminación legacy;
- resource topology físico del canonical Projection store;
- configuración física Root;
- contrato final Local/John/Jane.

## Siguiente frontera recomendada

Un único foco:

```text
ADMIN-UI-DRAFT-CUTOVER  PLANNED / NEXT
```

Objetivo: migrar callbacks, layout y browser draft store de Users Configuration al `UsersProfilesAdminDraft` / `UsersProfilesConfiguration` ya implementado, sin introducir todavía el wiring Manager exact-source ni tocar runtime/provenance.
