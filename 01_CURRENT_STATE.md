# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

Corte de implementación:
`moragaga/atlanticus@05d6cbb5b81b762f7fc06fc96b7959bfb835a7e3`.

## Resumen de estado

```text
WEB-STORAGE-TOPOLOGY                    CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY                  CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE         CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER            CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY       CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1                CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2            CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER          CLOSED / VERIFIED / CURRENT
PROFILES-DOMAIN-EXTRACTION              CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS             CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION               CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT          CLOSED / VERIFIED / CURRENT
USERS-PROFILES-DOMAIN-SEPARATION        IN PROGRESS
USERS-PROFILES-ADMIN-COMPOSITION        PLANNED / NEXT RECOMMENDED
USERS-RUNTIME-CANONICAL-CUTOVER         PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE  PLANNED
NAV-CONSUMER-MIGRATION-B                PLANNED
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

Después de UCS-1 una nueva exact Source release de Users Configuration contiene dos resources contractualmente separados:

```text
users/configuration.json.gz
profiles/configuration.json.gz
```

La release sigue siendo única y atómica desde la perspectiva de Source; no se creó un Source independiente de Profiles ni un segundo coordinator.

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
- campos Guest históricos no crean Profile funcional en la normalización.

Projection canónica vigente:

```text
ProjectionRecord[UsersProfilesConfiguration]
```

`UsersProfilesConfiguration` compone:
- `UsersConfiguration`;
- `ProfilesConfiguration`.

El Cosmos canonical Projection store escribe schema `2`, puede leer schema `1` y normaliza el payload histórico antes de aplicar las invariantes de exact target e idempotencia.

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
- cada Managed User, enabled o disabled, debe referenciar un Profile existente en el mismo payload mediante `profile_key`.

La regla anterior cierra la ambigüedad de orphan references en el contrato canónico. La UX/política administrativa para reasignar o impedir borrado sigue perteneciendo al siguiente hito administrativo.

## Legacy administrative configuration

El aggregate histórico `UsersConfigurationCatalog` y los servicios/bundle/callbacks administrativos existentes no fueron migrados por UCS-1.

Su shape histórico puede seguir conteniendo:

```text
administrator_background_color
administrator_text_color
guest_background_color
guest_text_color
profiles
users
```

Ese shape ya no es el contrato de escritura Source/Projection canónico nuevo.

La compatibilidad v1 es de lectura durable, no un shim runtime ni un nuevo contrato de authoring.

## Pending / Guest

Pending pertenece a Users, no a Profiles.

`EffectiveUser.profile` puede ser `None`.

Para `pending=True`:
- `enabled=True`;
- `profile is None`;
- `is_local=False`;
- no se aceptan overrides de avatar;
- colores estáticos actuales: fondo `#FF5722`, texto `#FFFFFF`.

Guest no es un Profile runtime ni un Profile funcional configurable en el nuevo contrato canónico.

## Administrator

Administrator es un Profile funcional normal.

En el contrato canónico nuevo aparece como `ProfileDefinition` explícito dentro de `ProfilesConfiguration`.

`UsersProfilesConfiguration` exige su presencia.

El aggregate legacy puede continuar sintetizándolo desde `administrator_*` mientras ese camino administrativo siga vigente.

## Root bootstrap

Root pertenece a Identity/bootstrap, no a Profiles ni al flujo normal Managed Users.

Implementado:
- `BootstrapRootPolicy`;
- `BootstrapRootAccessResolver`;
- `AccessDecision.bootstrap_root`;
- `AccessSnapshot.bootstrap_root`.

Contrato:

```text
policy.enabled
AND identity.issuer == policy.issuer
AND identity.subject_id == policy.subject_id
→ READY
→ bootstrap_root = True
→ user_id = None
```

`provider_key` no participa del match Root.

La fuente física de `BootstrapRootPolicy` y el mapping exacto Entra permanecen UNVERIFIED / OPEN.

## Service composition

Users no publica Profiles como servicio propio.

Contrato CURRENT:

```text
create_users_module(runtime)
→ registra únicamente USERS_RUNTIME_SERVICE_KEY
```

`PROFILE_CATALOG_SERVICE_KEY = "atlanticus.web.users.profiles"` permanece eliminado.

Esto no elimina la dependencia semántica legítima de `UsersAccessResolver` sobre `ProfileCatalog`.

## Local / John / Jane

Local, John y Jane quedan fuera de Profiles semánticos.

No deben reintroducirse como `ProfileDefinition` de sistema.

Su contrato runtime/ownership final permanece OPEN.

## Qualification UCS-1

Qualification ejecutada sobre el workspace real e integrada en `atlanticus:main`:

```text
focused UCS-1 tests       31 passed
Cosmos store hotfix test   9 passed
full Web suite            552 passed, 7 skipped
ruff check .              GREEN
compile productive/mirror GREEN
git diff --check          GREEN
```

No se afirma CI remoto adicional.

## Frontera Users / Profiles completa

Estado:

```text
USERS-PROFILES-DOMAIN-SEPARATION  IN PROGRESS
```

Cerrado por UCS-1:
- ownership durable contractual Users vs Profiles;
- representación canónica Administrator como Profile explícito;
- destino canónico de Guest fields: fuera del nuevo contrato;
- cross-contract orphan validation;
- Source único con dos resources;
- Projection payload compuesto separado;
- read-v1/write-v2 compatibility;
- Cosmos Projection read-v1/write-v2.

Permanece fuera:
- composición administrativa sobre contratos separados;
- runtime canonical cutover;
- provenance exact-release dentro de `users.runtime`;
- migración administrativa Users;
- eliminación legacy;
- resource topology físico del canonical Projection store;
- configuración física Root;
- contrato final Local/John/Jane.

## Siguiente frontera recomendada

Un único foco:

```text
USERS-PROFILES-ADMIN-COMPOSITION  PLANNED / NEXT RECOMMENDED
```

Debe migrar la composición administrativa para authoring conjunto sin recombinar ownership durable y sin tocar todavía runtime provenance.
