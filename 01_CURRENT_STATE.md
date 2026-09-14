# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

Corte de implementación:
`moragaga/atlanticus@3ca92d5579e499dd4ab6413fa6d91c9d296b13c2`.

## Resumen de estado

```text
WEB-STORAGE-TOPOLOGY                 CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY               CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE      CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER         CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY    CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1             CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2         CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER       CLOSED / VERIFIED / CURRENT
PROFILES-DOMAIN-EXTRACTION           CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS          CLOSED / VERIFIED / CURRENT
USERS-PROFILES-DOMAIN-SEPARATION     IN PROGRESS
NAV-CONSUMER-MIGRATION-B             PLANNED
USERS-CONTRACT-SEPARATION            PLANNED / NEXT RECOMMENDED
USERS-RUNTIME-CANONICAL-CUTOVER      PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE PLANNED
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

Users declara un único recurso durable confirmado:

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

## Source / Projection

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

`ProjectionRecord[UsersConfigurationCatalog]` continúa siendo el payload canónico vigente hasta un cutover explícito posterior.

El provenance legacy dentro de `users.runtime` todavía usa `projection_source_revision`; su migración exact-release sigue PLANNED.

## Manager

`MANAGER-ROOT-CANONICAL-CUTOVER` está CLOSED / VERIFIED / CURRENT.

La acción root Project:
- usa `ProjectionTarget`;
- selecciona current server-side;
- transporta el target exacto;
- no relee Source current durante `project(target)`;
- no obtiene la identidad ejecutable desde browser state.

Los contratos administrativos de publicación/verificación/history y el WORKSPACE IndexedDB continúan separados y pendientes.

## Profiles Domain Extraction

Implementado en:

```text
web/capabilities/profiles/core
```

Package:

```text
atlanticus-web-profiles==0.1.0
```

Estado:

```text
PROFILES-DOMAIN-EXTRACTION  CLOSED / VERIFIED / CURRENT
```

Ownership:
- `ProfileDefinition`, `ProfileCatalog`, normalizadores y `ProfilesDefinitionError` pertenecen a Profiles;
- Profiles no depende de Users;
- Users depende one-way de Profiles;
- `atlanticus.web.users.profiles` no existe como namespace Python productivo;
- no existe shim/re-export del namespace anterior.

## Profiles Baseline Semantics

Estado:

```text
PROFILES-BASELINE-SEMANTICS  CLOSED / VERIFIED / CURRENT
```

Subincrementos:

```text
PB-1 PROFILES-SEMANTIC-CORE                 CLOSED / VERIFIED / INTEGRATED
PB-2 DIRECT-CONSUMER-RECONCILIATION         CLOSED / VERIFIED / INTEGRATED
PB-3 ROOT-ACCESS-CONTRACT                    CLOSED / VERIFIED / INTEGRATED
PB-4 USERS-CONFIG-RECONCILIATION             ABSORBED BY PB-2 / CLOSED
PB-5 USERS-ADMIN-SEMANTIC-CLEANUP            ABSORBED BY PB-2 / CLOSED
PB-6 PROFILE-SERVICE-COMPOSITION-CLEANUP     CLOSED / VERIFIED / INTEGRATED
```

### ProfileCatalog

`ProfileCatalog` es ahora semánticamente puro:
- catálogo vacío significa vacío;
- sólo contiene `ProfileDefinition` explícitos;
- no fabrica Local, Administrator ni Guest;
- no posee defaults especiales;
- no posee `assignable()`;
- `administrator`, `guest`, `local` y `root` son strings ordinarios para Profiles core;
- duplicate normalized keys fallan;
- `require()` normaliza la key.

### Pending / Guest

Pending pertenece a Users, no a Profiles.

`EffectiveUser.profile` puede ser `None`.

Para `pending=True`:
- `enabled=True`;
- `profile is None`;
- `is_local=False`;
- no se aceptan overrides de avatar;
- colores estáticos actuales: fondo `#FF5722`, texto `#FFFFFF`.

`PendingUserRecord.to_effective_user()` no consulta Profiles.

Un Resolved User requiere Profile funcional y no puede usar `profile_key == "guest"`.

Guest deja de ser `ProfileDefinition` runtime.

### Administrator y Profiles funcionales

Administrator es un Profile funcional normal.

`UsersConfigurationCatalog.profile_catalog()` materializa:
- Administrator explícito;
- perfiles funcionales configurados.

No materializa Guest ni Local.

Antes de una primera proyección, `FileUsersProjectionProfileCatalog.all() == ()`.

Después de proyectar, expone únicamente Administrator + Profiles funcionales proyectados.

Managed Users referencian Profiles funcionales mediante `profile_key`.

### Durable Users Configuration

El shape durable vigente se preservó deliberadamente:

```text
administrator_background_color
administrator_text_color
guest_background_color
guest_text_color
profiles
users
```

Los campos Guest continúan round-trip por compatibilidad del contrato durable actual, pero no crean un Guest Profile runtime.

La separación durable Users/Profiles no fue ejecutada en este hito.

### Local / John / Jane

Local, John y Jane quedan fuera de Profiles semánticos.

No deben reintroducirse como `ProfileDefinition` de sistema.

Su contrato runtime/ownership final no fue definido por este hito y permanece OPEN.

### Root bootstrap

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

El match es exacto después del `strip()` normal de identidad; no se añadió `casefold()`.

`provider_key` no participa del match Root.

Si la policy está disabled o no coincide, el resolver delega al fallback normal.

Invariantes:
- `bootstrap_root=True` exige `READY`;
- `bootstrap_root=True` exige `user_id is None`;
- Root no se materializa como User ni Profile.

La sesión usa `_atlanticus_access_snapshot_v2`; snapshots previos no se adaptan.

La fuente física de `BootstrapRootPolicy` y el mapping exacto Entra permanecen UNVERIFIED / OPEN.

### Service composition

Users ya no publica Profiles como servicio propio.

Contrato CURRENT:

```text
create_users_module(runtime)
→ registra únicamente USERS_RUNTIME_SERVICE_KEY
```

`PROFILE_CATALOG_SERVICE_KEY = "atlanticus.web.users.profiles"` quedó eliminado.

Esto no elimina la dependencia semántica legítima de `UsersAccessResolver` sobre `ProfileCatalog`.

## Navigation

Navigation canonical Source, Projection Local/Cosmos y runtime canonical consumer están CLOSED / VERIFIED / CURRENT.

La migración administrativa permanece PLANNED.

Legacy deletion permanece BLOCKED hasta validar consumidores migrados.

## Alarm Engine

La qualification R3.5 permanece CLOSED PASS/GREEN.

Los contratos e invariantes específicos de Alarm siguen en `04_ALARM_ENGINE/`.

Este cierre de Profiles no reabre Alarm.

## Python / Trixie

Dirección decidida:
- Python 3.14.7;
- `python:3.14.7-slim-trixie`;
- `uv`, no pip como gestor normal.

La migración global 3.14.2 → 3.14.7 sigue fuera de este hito.

Estado:

```text
DECIDED / NOT YET IMPLEMENTED GLOBALLY
```

## Frontera Users / Profiles completa

Estado:

```text
USERS-PROFILES-DOMAIN-SEPARATION  IN PROGRESS
```

Aunque la baseline semántica está cerrada, permanecen fuera de este cierre:
- separación contractual/durable de Users y Profiles;
- eventual Source/Projection propia de Profiles si se justifica;
- política frente a Profile eliminado/no proyectado;
- runtime canonical cutover;
- provenance exact-release dentro de `users.runtime`;
- migración administrativa Users;
- eliminación legacy;
- configuración física Root;
- contrato final de Local/John/Jane.

## Siguiente frontera recomendada

Un único foco:

```text
USERS-CONTRACT-SEPARATION  PLANNED / NEXT RECOMMENDED
```

Debe definir contratos antes de consumidores y preservar los invariantes Source/Projection ya congelados.
