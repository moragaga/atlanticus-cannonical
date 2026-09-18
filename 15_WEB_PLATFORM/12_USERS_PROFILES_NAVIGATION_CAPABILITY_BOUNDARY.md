# Web Platform — Users / Profiles / Navigation Capability Boundary

Estado: **CURRENT DECISION / REFINED AFTER USERS GLOBAL REGISTRY CUTOVER**

## Propósito

Este documento fija la frontera CURRENT entre:

```text
Global Users
Profiles / Access application-specific
Navigation
```

sin reintroducir el modelo Users Configuration Source ya eliminado.

## Autoridad de implementación

CURRENT inspeccionado:

```text
moragaga/atlanticus:main
6dd09a6f24370bbad8ae358b6d5d7c6ea9aeba4a
```

Parent inmediato:

```text
4e008055ddc551e6c08a7d87715340c8c7cd149e
```

Git permanece SOLO LECTURA para el asistente.

## Estado del frente

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS

USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT

USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
PLANNED

ACCESS-PROFILES-CONFIGURATION
PLANNED

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
PLANNED
```

Quedan SUPERSEDED:

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
USERS-PROFILES-COMPOSITION-CUTOVER as previous Source-centric model
```

## Regla principal

Atlanticus es una base genérica.

Global Users debe ser reusable y no conocer aplicaciones concretas.

```text
Global Users
    identity + lifecycle + global base authority
```

Profiles/Access pertenecen a la aplicación que los define.

```text
Application
    Profiles
    Access
    Navigation configuration
    domain capabilities
```

No introducir estado app-specific dentro del Global User.

## Users

Users puede existir standalone.

CURRENT estructura:

```text
web/capabilities/users/
├── activity
├── blob
├── core
└── cosmos
```

No existen CURRENT:

```text
users/configuration
users/projection-cosmos
users-manager composition
```

### Strong identity

```text
issuer
subject_id
user_id = build_user_key(issuer, subject_id)
```

Strong identity es la frontera primaria de identidad.

Email/display name no autorizan merge automático entre strong identities distintas.

### UserRecord

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

No contiene:

```text
profile_key
app_id
navigation role
ADA Access
Tools/KPI configuration
```

### Authorities

Managed global:

```text
basic
root
```

Runtime local:

```text
local
```

`administrator` no pertenece a Users CURRENT.

`guest` ya no pertenece al Users authority contract CURRENT.

No existe mapping:

```text
administrator -> root
```

### Local identities

```text
Jane Doe
local
#C85D91 / #FFFFFF

John Doe
local
#3778C2 / #FFFFFF
```

Jane y John son identities locales, no Profiles.

El wiring exacto del selector local en todas las compositions sigue UNVERIFIED.

## Users Registry

Durable contract:

```text
UsersRegistryStore
```

Provider CURRENT:

```text
BlobUsersRegistryStore
```

Default blob:

```text
users/users.json.gz
```

Documento:

```text
document_type = atlanticus_users_registry
schema_version = 1
```

El container se inyecta desde afuera del dominio.

No hardcodear container productivo en Users core/provider salvo contrato real.

## Promoted Users / runtime

Provider CURRENT:

```text
CosmosUsersStore
```

Documento:

```text
document_type = atlanticus_user
schema_version = 1
```

Login:

```text
resolve only
no observe
no pending write
```

Absent promoted user:

```text
USER_NOT_PROMOTED
```

Legacy documents `pending` / `resolved` no son aceptados por el store CURRENT.

## Administration lifecycle

Core:

```text
UsersAdministrationService
UsersAdministrationStore
UsersRegistryStore
UsersDirectoryReader
```

Candidate states:

```text
PROMOTABLE
CONFLICT
PROMOTED
```

### Candidate rules

```text
Cosmos present
=> PROMOTED

Registry only
=> PROMOTABLE

Directory only
=> PROMOTABLE

Registry + Directory, exact same represented data
=> PROMOTABLE

Registry + Directory, differing represented data
=> CONFLICT

same email across different strong identities
=> non-promoted candidate CONFLICT / promotion blocked
```

Promoted Users pueden exponer issues de inconsistencia sin dejar de ser PROMOTED.

### Promotion ordering

```text
1. reject already promoted
2. load/validate Registry + Directory candidate
3. persist Registry when needed using expected version
4. create Cosmos promoted User
```

No distributed transaction.

Si step 3 pasa y step 4 falla:

```text
Registry yes
Cosmos no
```

Ese estado debe poder repararse/reintentarse; no se revierte automáticamente Blob.

## Profiles

Profiles es first-class capability application-specific.

CURRENT:

```text
profiles/core
profiles/configuration
```

Ownership:

```text
profiles/core
ProfileDefinition
ProfileCatalog
profile domain invariants

profiles/configuration
ProfilesConfiguration
```

`ProfilesConfiguration` no vive dentro de core.

Target posterior todavía pendiente:

```text
Profiles independent Source/Projection lifecycle where justified by current contracts
Profiles administration
Profiles UI
```

No mover Global Users lifecycle a Profiles.

## Access

Access es application-specific.

Target conceptual:

```text
Promoted Global User
        ↓ application association
Profile / Access
        ↓
effective app capabilities
```

El owner y shape exactos de esa association permanecen:

```text
OPEN / UNVERIFIED
```

No inventar contract, store o package desde este documento.

## Navigation

Navigation permanece configuration domain.

El diagrama canónico anterior:

```text
Users
  ↓
Profiles
  ↓
Navigation
```

queda refinado porque Users ya no es Configuration Source.

Target conceptual vigente:

```text
Global Users
    ↓ resolved by app composition
Profile / Access effective state
    ↓
Navigation
```

Navigation core debe permanecer desacoplado cuando sea posible.

Preferir un boundary efectivo de autorización/perfil provisto por composition antes
que importar lifecycle/storage de Users.

La forma final queda:

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
PLANNED
```

## Source / Projection

Aplica a Profiles/Navigation cuando esas capabilities sean configuration lifecycle.

No aplica a Global Users CURRENT.

La propuesta anterior:

```text
Users
source_key = users
payload = UsersConfiguration
```

queda:

```text
SUPERSEDED / REMOVE FROM CANONICAL TARGET
```

No crear otro Users Source para restaurar simetría visual con Profiles/Navigation.

## UI

Legacy Users configuration UI fue eliminada con `users/configuration`.

No existe nueva Users Administration UI en este hito.

Target futuro:

```text
Users Administration surface
consumes UsersAdministrationService
```

Profiles UI deberá ser Profiles-owned cuando se implemente.

Navigation UI permanece Navigation-owned.

No mezclar estas tres superficies sólo porque aparezcan dentro de una misma aplicación.

## Persisted data

Código CURRENT ya no lee legacy schema.

Persisted data real no fue migrado por este hito.

Siguiente frontera:

```text
USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```

No borrar datos antiguos hasta:

```text
inventory verified
Global Users extracted
Profiles information preserved where needed
new Blob/Cosmos state verified
explicit delete criteria satisfied
```

No implementar old-schema reader en runtime como transición.

## Testing rules

Automatizar:

```text
behavior
contracts
invariants
regressions
critical flows
```

No automatizar como contract tests:

```text
CSS visual
responsive
spacing
branding
source token scans
import scans
existence/non-existence of functions/classes
AST/module structure
implementation internals
```

## Reglas congeladas

```text
Atlanticus generic
REQUIRED

Global Users standalone
REQUIRED

Global Users app-specific state
FORBIDDEN

Global User strong identity
issuer + subject_id

Managed authority
basic | root

local
LOCAL-RUNTIME ONLY

administrator
REMOVED FROM USERS

guest
REMOVED FROM USERS AUTHORITY CONTRACT

Users Configuration Source
REMOVED

Users generic Projection
REMOVED

Users Manager module
REMOVED

Users login write/pending
FORBIDDEN

Blob registry default path
users/users.json.gz

Cosmos current document
atlanticus_user schema 1

Users Registry document
atlanticus_users_registry schema 1

OLD SCHEMA RUNTIME READERS
FORBIDDEN

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN

Profiles
APPLICATION-SPECIFIC FIRST-CLASS CAPABILITY

Access
APPLICATION-SPECIFIC

Global User -> app Profile/Access exact association
OPEN / DO NOT INVENT
```

## Orden de implementación refinado

```text
1. USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
   CLOSED / VERIFIED / CURRENT

2. USERS-PERSISTED-DATA-CUTOVER
   PLANNED / NEXT

3. USERS-ADMINISTRATION-SURFACE-CUTOVER
   PLANNED

4. PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
   PLANNED

5. ACCESS-PROFILES-CONFIGURATION
   PLANNED

6. NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
   PLANNED
```

Los puntos 3-6 no deben adelantarse dentro del punto 2.

## Pendientes explícitos

```text
actual persisted data inventory
OPEN / NEXT

legacy persisted data migration/deletion
OPEN / NEXT

concrete Entra/Graph Directory provider
UNVERIFIED

Users Administration UI/repair
PLANNED

Profiles lifecycle/admin/UI
PLANNED

Global User -> app Profile/Access association
OPEN / UNVERIFIED

Navigation effective-access integration
PLANNED

local selector composition wiring
UNVERIFIED

Python 3.14.7 metadata alignment
OPEN / SEPARATE

WEB-TEST-CONTRACT-CLEANUP
OPEN / SEPARATE
```
