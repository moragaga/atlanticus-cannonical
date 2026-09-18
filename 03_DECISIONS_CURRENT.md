# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / LOCALLY USED / METADATA NOT YET GLOBALLY ALIGNED |
| `python:3.14.7-slim-trixie` | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `uv`, no pip normal | CURRENT |
| Definir contratos antes que consumidores | CURRENT |
| Backend antes que frontend | CURRENT |
| Cutover raíz limpio | CURRENT |
| No crear shims/adapters/aliases temporales para legacy | FROZEN |
| No conservar doble contrato | FROZEN |
| Tests no son autoridad sobre contratos SUPERSEDED | FROZEN |
| Un consumer puede quedar temporalmente roto durante un root cutover | FROZEN |

## Regla universal de cutover

```text
LEGACY
REMOVE

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOBLE CONTRATO
FORBIDDEN

OLD SCHEMA READERS IN CURRENT RUNTIME
FORBIDDEN

CONTRATO FINAL
Responsabilidad real del dominio; infraestructura genérica sólo donde aplique
```

Usar infraestructura genérica no cambia automáticamente ownership de dominio.

No forzar Source/Projection/Manager sobre una entidad que no sea configuration lifecycle.

## Source / Projection

| Decisión | Estado |
|---|---|
| Source genérico pertenece a `web/capabilities/source` | CURRENT |
| Projection exact-release pertenece a `web/capabilities/projection/core` | CURRENT |
| Release identity != content hash | FROZEN |
| Source current nunca lo determina Cosmos | FROZEN |
| Projection target = `SourceKey + SourceReleaseRef + dependencies` | FROZEN |
| Projection dependencies son exact `ProjectionTarget` | FROZEN / IMPLEMENTED |
| `project(target)` no relee current | FROZEN |
| CURRENT/OUTDATED compara exact target | FROZEN |
| No reconstruir `ProjectionTarget` desde revision | FROZEN |
| `expected_source_revision` | SUPERSEDED / REMOVED |
| private projection revision identity | SUPERSEDED / REMOVED |

Estas decisiones aplican a configuration domains. Users CURRENT no es uno de ellos.

## Manager generic contract

| Decisión | Estado |
|---|---|
| Manager tiene un solo contrato Source/Projection genérico | FROZEN / IMPLEMENTED |
| `ManagerModule.source_key` | FROZEN / IMPLEMENTED |
| `ManagerModule.source_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.source_reader_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.projection_service` | FROZEN / IMPLEMENTED |
| `ManagerModule.draft_validation_service` | FROZEN / IMPLEMENTED |
| `source_history_service` opcional | FROZEN / IMPLEMENTED |
| `workflow_service` legacy | SUPERSEDED / REMOVED |
| campos `exact_source_*` | SUPERSEDED / REMOVED |
| `exact_projection_service` | SUPERSEDED / REMOVED |
| doble routing exact/legacy | FORBIDDEN |
| adapters/shims/aliases para conservar contrato anterior | FORBIDDEN |

## Manager invariants

```text
Source BASE = SourceSnapshot
Workspace identity != Source identity
ProjectionTarget llega completo a project(...)
Manager no reconstruye Source/Projection identity desde revision strings
History usa HistoryPage + SourceReleaseRef
```

Estado: **FROZEN / CURRENT**.

## Users global registry

Estado:

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

### Ownership

Users es registry/lifecycle de entidad global.

```text
Users != Configuration Source
Users != Profile assignment
Users != Access configuration
Users != Navigation configuration
```

No reintroducir:

```text
UsersConfiguration
UsersProfilesConfiguration
UsersProfilesAdministrationService
UsersProfilesAdminDraft
Users Source
Users Projection
Users Manager Source workflow
```

### Strong identity

Contrato congelado:

```text
strong identity = (issuer, subject_id)
user_id = build_user_key(issuer, subject_id)
```

Mismo email/display name no permite fusionar strong identities distintas.

### Global User shape

Campos CURRENT:

```text
user_id
issuer
subject_id
display_name
email
enabled
authority_key
avatar_background_color
avatar_text_color
```

No añadir estado app-specific al Global User.

### Authorities

Contrato CURRENT:

```text
basic
ASSIGNABLE / STANDARD

root
ASSIGNABLE / FULL AUTHORITY

local
LOCAL-RUNTIME ONLY / NON-ASSIGNABLE / FULL AUTHORITY
```

Quedan SUPERSEDED:

```text
guest as Users authority
administrator
administrator -> root mapping
functional app profile keys inside Global User authority
```

Jane Doe y John Doe siguen siendo identidades locales, no Profiles.

Colores preservados:

```text
Jane Doe
#C85D91 / #FFFFFF

John Doe
#3778C2 / #FFFFFF
```

### Runtime login

Contrato congelado:

```text
UsersRuntimeStore.resolve(identity)
READ ONLY
```

Si no existe promoted User:

```text
AccessStatus.USER_NOT_PROMOTED
```

Login no crea `pending`, no llama `observe()` y no muta durable state.

### Durable registry

Contrato:

```text
UsersRegistryStore.load()
UsersRegistryStore.replace(users, expected_version)
```

Provider CURRENT:

```text
BlobUsersRegistryStore
blob_name default = users/users.json.gz
```

Documento:

```text
document_type = atlanticus_users_registry
schema_version = 1
```

Concurrencia por ETag:

```text
first create -> overwrite=False
replace      -> If-Match expected ETag
read         -> ETag stable before/after download
```

No convertir ETag en Source release identity.

### Promoted store

Provider CURRENT:

```text
CosmosUsersStore
```

Documento:

```text
document_type = atlanticus_user
schema_version = 1
id = user_id
```

No aceptar documentos legacy `pending` / `resolved`.

### Candidate states

```text
PROMOTABLE
CONFLICT
PROMOTED
```

Cosmos presence implica already promoted para promotion.

Registry + Directory con datos distintos para la misma strong identity se presenta
como conflict que requiere resolución explícita; no se fusiona silenciosamente.

Email compartido por strong identities distintas bloquea promotion automática.

### Promotion ordering

Congelado:

```text
1. comprobar promoted store
2. leer/validar Registry + optional Directory candidate
3. persistir Registry con expected version cuando cambia
4. crear promoted User en Cosmos
```

Si Registry write pasa y Cosmos create falla:

```text
NO rollback distributed transaction
Registry yes / Cosmos no
retry/repairable state
```

## Profiles / Access

Profiles permanece first-class capability:

```text
profiles/core
profiles/configuration
```

Profiles y Access son application-specific.

Regla congelada:

```text
Global Users no conoce aplicaciones concretas.
```

El contrato final de asociación:

```text
Global User -> app-specific Profile/Access
```

permanece OPEN. No inventarlo desde Users core.

La secuencia canónica anterior `Users Source -> Profiles Source -> Navigation` queda
refinada: Users ya no participa como Source.

## Users / Profiles combined contracts

```text
UsersProfilesConfiguration
SUPERSEDED / REMOVED

UsersProfilesAdministrationService
SUPERSEDED / REMOVED

UsersProfilesAdminDraft
SUPERSEDED / REMOVED

Profiles resource inside Users Source
SUPERSEDED / REMOVED

Profiles UI inside legacy Users configuration UI
SUPERSEDED / REMOVED WITH LEGACY PACKAGE
```

No recrear atomicidad mediante transacción distribuida.

## ADA Configuration Manager

Users no es módulo del Configuration Manager CURRENT.

La composition administra sólo Configuration domains presentes:

```text
Navigation
Tools
KPI Configuration optional
KPI Definition optional
```

Una futura Users Administration surface debe consumir el lifecycle de Users, no
simular Source/Projection para volver a entrar al Manager genérico.

## Testing

Tests protegen:

```text
behavior
contracts
invariants
regressions
critical flows
```

No crear tests cuyo único objetivo sea:

```text
CSS visual
spacing
responsive
branding
apariencia
estructura visual
existencia/no existencia de funciones o clases
source-token scans
import scans
AST/module structure
detalles internos de implementación
```

## Estado de ejecución

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS

USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```

## Decisiones reemplazadas o refinadas

1. Users como generic Configuration Source.
   → **SUPERSEDED**; Users es global entity registry/lifecycle.

2. `USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER` separando Users Source y Profiles Source.
   → **SUPERSEDED / NOT FINAL TARGET**; Users Source fue eliminado.

3. `UsersProfilesConfiguration` como agregado de administración.
   → **SUPERSEDED / REMOVED**.

4. `UserConfiguration.profile_key -> authority_key` dentro de un Users Source final.
   → **SUPERSEDED AS LAYER**; Global `UserRecord.authority_key` existe, pero no dentro de Users Configuration.

5. `guest` como authority base CURRENT.
   → **SUPERSEDED / REMOVED FROM USERS AUTHORITY CONTRACT**.

6. `administrator` dentro del boundary Users/Profiles.
   → **SUPERSEDED / REMOVED FROM USERS**; no alias hacia root.

7. Users dentro de ADA Configuration Manager.
   → **SUPERSEDED / REMOVED**.

8. Profiles bajo package genérico `management`.
   → **SUPERSEDED / NOT ADOPTED**.

9. `ProfilesConfiguration` dentro de `profiles/core`.
   → **SUPERSEDED / REMOVED**; owner CURRENT `profiles/configuration`.

## Qualification del checkpoint

Implementación CURRENT:

```text
moragaga/atlanticus@6dd09a6f24370bbad8ae358b6d5d7c6ea9aeba4a
```

Parent:

```text
4e008055ddc551e6c08a7d87715340c8c7cd149e
```

Evidencia observada:

```text
Python 3.14.7 local qualification
uv lock PASS
uv sync PASS
web pytest 416 PASS / 7 SKIPPED
Users/Identity scoped Ruff PASS
ADA Configuration Manager scoped qualification PASS / user-observed
```

No atribuir PASS a full Ruff workspace, CI remoto o persisted-data migration.

## Conflicto abierto de Python metadata

Decisión global:

```text
Python 3.14.7
```

`web/pyproject.toml` CURRENT todavía declara:

```text
requires-python = "==3.14.2"
```

Permanece OPEN y fuera del siguiente incremento.

## Siguiente foco único

```text
USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```
