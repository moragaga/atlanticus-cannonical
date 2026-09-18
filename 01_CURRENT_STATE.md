# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@6dd09a6f24370bbad8ae358b6d5d7c6ea9aeba4a
```

Parent inmediato:

```text
4e008055ddc551e6c08a7d87715340c8c7cd149e
```

Canonical inspeccionado para este cierre:

```text
moragaga/atlanticus-cannonical@61da5829c6a1f8ec936d46e5a7ec02965b5e4743
```

Git permanece SOLO LECTURA para el asistente.

## Estado resumido

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER           CLOSED / VERIFIED / CURRENT
PROFILES-CONFIGURATION-BOUNDARY-CUTOVER      CLOSED / VERIFIED / CURRENT
PROFILES-CAPABILITY-EXTRACTION               IN PROGRESS
USERS-PERSISTED-DATA-CUTOVER                 PLANNED / NEXT
USERS-ADMINISTRATION-SURFACE-CUTOVER         PLANNED
PROFILES-INDEPENDENT-SOURCE-LIFECYCLE        PLANNED
ACCESS-PROFILES-CONFIGURATION                PLANNED
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT     PLANNED
WEB-TEST-CONTRACT-CLEANUP                    PLANNED / OPEN
PYTHON-METADATA-ALIGNMENT                    PLANNED / OPEN
```

Los hitos genéricos de Manager, Navigation, Tools, KPI Configuration,
KPI Definition y ADA Configuration Manager cerrados anteriormente permanecen
`CLOSED / VERIFIED / CURRENT`.

## VERIFIED

### Published checkpoint

`main` está publicado en:

```text
6dd09a6f24370bbad8ae358b6d5d7c6ea9aeba4a
```

con parent inmediato:

```text
4e008055ddc551e6c08a7d87715340c8c7cd149e
```

### Users global registry root cutover

El árbol CURRENT de Users contiene:

```text
web/capabilities/users/activity
web/capabilities/users/blob
web/capabilities/users/core
web/capabilities/users/cosmos
```

Ya no existen en `main`:

```text
web/capabilities/users/configuration
web/capabilities/users/projection-cosmos
web/compositions/users-manager
```

El antiguo `users/core/.../profiles.py` comentado también fue removido.

### Global User contract

`UserRecord` representa un User global promovido/durable con:

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

Invariante de identidad:

```text
user_id = build_user_key(issuer, subject_id)
```

Managed Users aceptan únicamente:

```text
basic
root
```

`local` permanece reservado para runtime local y no es asignable a Users manejados.

El contrato CURRENT ya no define `guest` como authority de Users.

### Runtime login

`UsersRuntimeStore` expone sólo:

```text
resolve(identity) -> UserRecord | None
```

No existe `observe()` ni escritura pending durante login.

Cuando la identidad autenticada no está promovida:

```text
AccessStatus.USER_NOT_PROMOTED
```

La decisión incluye el `user_id` determinístico.

### Administration lifecycle

Contratos CURRENT:

```text
UsersAdministrationStore
UsersRegistryStore
UsersDirectoryReader
UsersAdministrationService
```

Estados de candidate:

```text
PROMOTABLE
CONFLICT
PROMOTED
```

Promotion CURRENT:

```text
1. verificar que Cosmos no tenga el User promovido
2. descubrir/validar candidato Registry/Directory
3. escribir Registry con control de versión cuando corresponda
4. crear User en Cosmos
```

No existe rollback de Blob si la creación posterior en Cosmos falla.
El estado durable-registry presente / Cosmos ausente queda recuperable mediante
reintento o futura superficie de reparación; no se introduce adapter legacy.

### Blob Users Registry

Provider CURRENT:

```text
atlanticus-web-users-blob
BlobUsersRegistryStore
```

Default provider-relative blob name:

```text
users/users.json.gz
```

El container se inyecta; no está hardcodeado por Users.

Documento:

```text
document_type = atlanticus_users_registry
schema_version = 1
```

Concurrencia:

```text
first create -> upload(overwrite=False)
existing     -> upload_if_match(expected ETag)
read         -> ETag before/after must match
```

### Cosmos promoted store

Provider CURRENT:

```text
CosmosUsersStore
```

Documento final:

```text
document_type = atlanticus_user
schema_version = 1
id = user_id
partition key value = user_id
```

No existe reader CURRENT para documentos legacy `pending` / `resolved`.

### ADA Configuration Manager

Users fue removido del Configuration Manager.

La superficie CURRENT registra:

```text
navigation
tools
kpis                 optional
kpi-definitions      optional
```

No existen Users service keys, Users Source workflow, Users Projection ni Users
admin module dentro de esta composition.

### Qualification observada

En entorno local Fedora/WSL con Python 3.14.7:

```text
uv lock
PASS

uv sync
PASS

web workspace pytest
416 passed / 7 skipped

Ruff scoped
capabilities/identity/core
capabilities/users/core
capabilities/users/blob
capabilities/users/cosmos
PASS
```

La qualification final del package `ada-configuration-manager` fue reportada por
el usuario como OK después de alinear el test stale de índices y los dos findings
E731 de `composition.py`.

El commit remoto CURRENT contiene esas correcciones.

## INFERRED

La ausencia de Users Configuration/Projection/Manager del árbol y del workspace,
combinada con los contratos `UsersRegistryStore` + `CosmosUsersStore`, demuestra
que Users dejó de pertenecer al lifecycle genérico Source/Projection.

Esto no convierte Profiles ni Access en parte del registry global de Users.

La existencia de issues de candidate para Registry/Cosmos/Directory permite una
futura superficie de reparación, pero esa UI/operación no está implementada.

## ASSUMED

No se asume:

- contenido real de Blob/Cosmos en producción;
- que exista actualmente `users/users.json.gz` en el container objetivo;
- que documentos Cosmos legacy hayan sido migrados o borrados;
- ubicación/container/productive connection exactos para Users Registry;
- concrete Microsoft Graph/Entra directory listing provider;
- ownership final del vínculo global User → app-specific Profile/Access;
- que Profiles lifecycle independiente esté implementado;
- que Navigation alignment esté implementado;
- que CI remoto haya corrido;
- que full Ruff workspace esté limpio;
- que metadata `requires-python` esté alineada globalmente con 3.14.7.

## PROPOSED

Único foco siguiente:

```text
USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```

Debe empezar por evidencia real de datos/topología y definir una migración one-shot
sin lectores legacy runtime.

No implementar todavía Users Administration UI, Profiles, Access o Entra provider
como parte de ese foco.

## SUPERSEDED / REFINED

### Users como configuration Source

El target anterior:

```text
Users Source
UsersConfiguration only
```

queda:

```text
SUPERSEDED
```

Users CURRENT es lifecycle de entidad/registry global, no configuration Source.

### Users / Profiles combined Source ownership cutover

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
SUPERSEDED / NOT EXECUTED AS FINAL TARGET
```

El agregado combinado fue eliminado de raíz en lugar de dividir Users en otro Source.

### Users Profiles composition cutover

El target previo que hacía de Users/Profiles una cadena de configuration Source
queda `SUPERSEDED`.

Refinamiento CURRENT:

```text
Global Users
independent registry/entity lifecycle

Profiles / Access
application-specific configuration and association
```

El contrato exacto de asociación sigue OPEN y no debe inventarse.

### Users authority baseline anterior

El contrato previo que incluía `guest` queda refinado.

CURRENT:

```text
basic   managed / assignable
root    managed / assignable / full access
local   runtime-local only / non-assignable
```

No existe `administrator -> root` ni `guest` managed compatibility.

## UNVERIFIED / OPEN

### Persisted data

No se ha inspeccionado en este cierre el contenido real de:

```text
legacy Users/Profiles Source data
legacy Users Cosmos pending/resolved documents
current production Users Cosmos documents
current production Blob registry
```

No borrar datos persistidos antes de inventariar y preservar la información que
todavía pertenezca a Profiles.

### Entra directory discovery

Existe el boundary genérico:

```text
UsersDirectoryReader
```

No existe evidencia CURRENT de un provider reutilizable concreto que liste el
directorio Entra/Graph.

Estado:

```text
UNVERIFIED / NOT IMPLEMENTED IN THIS HITO
```

### Users Administration surface

No existe una nueva UI de Users en este hito.

Estado:

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED
```

### Profiles / Access

`profiles/core` y `profiles/configuration` permanecen CURRENT.

No está implementado todavía el lifecycle Source/Projection/admin/UI independiente
completo de Profiles ni el contrato final de Access/Profile association.

### Python metadata

Baseline global:

```text
Python 3.14.7
python:3.14.7-slim-trixie
```

`web/pyproject.toml` CURRENT todavía declara:

```text
requires-python = "==3.14.2"
```

La qualification local sí se ejecutó bajo Python 3.14.7, pero la metadata permanece
inconsistente.

### Full Ruff / CI

Ruff completo del workspace no se declara PASS en este cierre.
Antes del cleanup scoped se observaron findings fuera del incremento en Manager y
Navigation; no se corrigieron oportunistamente.

GitHub no reporta status checks ni workflow runs asociados al commit CURRENT.

## Conflictos canonical detectados

El canonical `61da5829...` quedó desactualizado respecto de implementación CURRENT.

Conflictos principales:

```text
canonical: Users es Source/configuration
CURRENT:   Users configuration/projection/users-manager fueron eliminados

canonical: UsersProfilesConfiguration todavía CURRENT
CURRENT:   package users/configuration eliminado

canonical: guest authority transitional
CURRENT:   guest ya no existe en Users authority contract

canonical: next = USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
CURRENT:   ese target fue superado por global registry lifecycle
```

Los replacements de este cierre deben actualizar como mínimo:

```text
00_AUTHORITY.md
00_INDEX.md
01_CURRENT_STATE.md
02_ARCHITECTURE.md
03_DECISIONS_CURRENT.md
07_VALIDATION_BASELINE.md
08_ROADMAP.md
09_OPEN_QUESTIONS.md
15_WEB_PLATFORM/00_INDEX.md
15_WEB_PLATFORM/01_CAPABILITY_INDEPENDENCE.md
15_WEB_PLATFORM/09_CURRENT_GAPS.md
15_WEB_PLATFORM/11_OPEN_ITEMS.md
15_WEB_PLATFORM/12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md
```

## Siguiente frontera

```text
USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```

No mezclar:

```text
Users admin UI
Profiles lifecycle
Access
Navigation alignment
Python metadata
Web test cleanup
Command Center
unrelated Ruff cleanup
```
