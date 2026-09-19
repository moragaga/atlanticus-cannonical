# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA.

## Ownership y scopes

`scopes/` contiene composiciones y capacidades específicas de un producto/proyecto cuando corresponde.

Una capability bajo `scopes/ada` puede consumir infraestructura genérica Atlanticus sin transferir su ownership al core genérico.

Regla CURRENT:

```text
Atlanticus generic infrastructure/capabilities
    Source / Projection / Manager / Navigation / Users / Profiles / ...

ADA-specific capabilities
    Tools / KPI Configuration / KPI Definition / ADA Access / ...
```

No generalizar una capability sólo porque reutiliza contratos genéricos.

## Planos principales

### Platform

Capacidades transversales:

- backend;
- connectivity;
- integrations;
- web.

`backend/` representa backend jobs y capacidades propias de esos jobs.

`web/` es frontera de primer nivel para Flask/Dash, JavaScript/CSS, composición Web,
server-side Python con responsabilidad Web y capabilities Web reutilizables.

Connectivity es dual-use y no adquiere ownership funcional.

### Configuration / Administration

Manager administra configuración, authoring, validation, publication, history y
projection actions para dominios que realmente sean Configuration Sources.

Source genérico pertenece a:

```text
web/capabilities/source/
```

Projection genérica exact-release pertenece a:

```text
web/capabilities/projection/core
```

Manager consume estos contratos genéricos directamente. No mantiene una arquitectura
paralela `legacy` vs `exact`.

No toda entidad administrable debe convertirse en Manager/Source/Projection.

### Entity lifecycle

Users CURRENT pertenece a un lifecycle de entidad global, no a Configuration Source.

```text
Global Users Registry
        │
        ├── durable registry: UsersRegistryStore / Blob provider
        ├── promoted/runtime store: UsersAdministrationStore + UsersRuntimeStore / Cosmos
        └── optional directory discovery: UsersDirectoryReader
```

Esto es una frontera diferente de:

```text
SourceRelease
ProjectionTarget
ManagerModule
```

### Operational Data

Operational Data conserva ownership separado para sources, producers, processes,
planner y materialization.

### ADA Runtime

ADA Generic compone la experiencia operacional y consume capacidades Atlanticus y
contratos ADA-specific ya resueltos.

ADA-specific authorization puede consumir contratos genéricos, pero no convertirse en
dependencia del core Atlanticus.

## Manager vs ADA Generic

```text
Manager      = administrar configuración Source/Projection
ADA Generic  = consumir configuración y materializar experiencia operacional
Users Admin  = administrar lifecycle de Users globales
```

No fusionar estas responsabilidades por conveniencia de UI.

## Configuration vs Data

```text
CONFIGURATION DETERMINES EXISTENCE
DATA DETERMINES STATE
ENTITY REGISTRY DETERMINES GLOBAL USER LIFECYCLE
```

## Source vs Projection

Source y Projection son responsabilidades separadas.

```text
Source     = Local | Blob
Projection = Local | Cosmos | provider equivalente
```

Projection representa un `SourceReleaseRef` concreto mediante `ProjectionTarget`.

Source current nunca se determina desde Cosmos.

`ProjectionTarget.dependencies` representa dependencias semánticas exactas entre
projections cuando existen realmente.

No existe un orden global obligatorio de todas las proyecciones.

Estas reglas siguen CURRENT para dominios de Configuration; no se aplican a Users
sólo por analogía.

## Contrato único de Manager

Cada `ManagerModule` declara:

```text
SourceKey
source_service
source_reader_service
projection_service
draft_validation_service
source_history_service | None
```

No existe una segunda familia `exact_*`.

### Source

```text
SourceReaderWorkflow
SourcePublicationWorkflow
SourceHistoryWorkflow
```

Todos transportan modelos de `source/core`.

### Projection

Manager consume:

```text
get_status(source_key)
select_current_target(source_key)
project(ProjectionTarget)
```

No existe adapter Manager hacia una identidad textual de revisión.

## Workspace genérico

`ManagerWorkspace` conserva:

```text
owner
payload local
SourceSnapshot como BASE
local revision
base payload revision
saved_at
```

Reglas:

- local revision identifica payload local;
- Source release identity permanece en `SourceSnapshot`;
- concurrency token no se convierte en release identity;
- no reconstruir `ProjectionTarget` desde revision.

## Navigation CURRENT

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

Navigation continúa como configuration domain genérico.

Autorización core:

```text
principal.unrestricted
OR
principal.access_key in allowed_profiles
```

Durable configuration:

```text
allowed_profiles = tuple de profile keys
```

Navigation Configuration puede consumir Profiles core para catálogo y validación:

```text
ProfileCatalog
ProfileDefinition
NavigationProfileCatalogProvider
```

No depende de:

```text
Users
ADA Access
Profiles Configuration
```

El mini-modelo local `NavigationProfileOption/_BASE_PROFILES` fue eliminado.

## Users CURRENT

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Estructura publicada:

```text
users/
├── activity
├── blob
├── core
└── cosmos
```

No existen CURRENT:

```text
users/configuration
users/projection-cosmos
compositions/users-manager
```

### Global identity

Strong identity:

```text
(issuer, subject_id)
user_id = build_user_key(issuer, subject_id)
```

Un `UserRecord` global no contiene app profile, Navigation, Tools, KPI ni otra
configuración de una aplicación específica.

### Authorities

Managed global Users:

```text
basic
root
```

Runtime local:

```text
local
```

No existe compatibility alias `administrator -> root`.

### Durable registry

`UsersRegistryStore` es contrato durable de registry.

Provider Blob CURRENT:

```text
BlobUsersRegistryStore
users/users.json.gz
atlanticus_users_registry / schema 1
```

El container es configuración inyectada.

### Promoted/runtime state

Cosmos CURRENT:

```text
CosmosUsersStore
atlanticus_user / schema 1
```

Login consulta sólo promoted state y no escribe pending.

Identity autenticada sin promoted record:

```text
READY
```

Promoted disabled:

```text
USER_DISABLED / 403
```

### Administration

`UsersAdministrationService` compone:

```text
UsersRegistryStore
UsersAdministrationStore
UsersDirectoryReader | None
```

Candidate state:

```text
PROMOTABLE
CONFLICT
PROMOTED
```

No se introduce rollback distribuido ni adapter legacy.

## Profiles CURRENT

```text
PROFILES-CAPABILITY-EXTRACTION
CLOSED / VERIFIED / CURRENT

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
CLOSED / VERIFIED / CURRENT
```

Estructura:

```text
profiles/core
profiles/configuration
```

Ownership:

```text
profiles/core
ProfileDefinition
ProfileCatalog

profiles/configuration
ProfilesConfiguration
Profiles Source lifecycle
```

Profiles es generic Atlanticus first-class capability.

No agregar campos ADA-specific al modelo generic.

El vínculo entre Global User y estado application-specific permanece fuera de Users core.

## ADA Access CURRENT

```text
ADA-ACCESS-PROFILES-CONFIGURATION
CLOSED / VERIFIED / CURRENT
```

ADA Access es application-specific bajo `scopes/ada`.

Puede consumir `ProfileCatalog` para validar sus referencias.

Navigation no depende de ADA Access.

## Tools CURRENT

Ownership:

```text
scopes/ada/web/tools
```

Tool Configuration conserva semántica ADA y consume infraestructura genérica Source/Projection.

```text
TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## KPI Configuration CURRENT

Ownership:

```text
scopes/ada/web/kpis/configuration
```

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Dependencia exacta:

```text
Tool ProjectionTarget
        ↓ dependency
KPI Configuration ProjectionTarget
```

## KPI Definition CURRENT

Ownership:

```text
scopes/ada/web/kpis/definition
```

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Dependencia exacta:

```text
KPI Configuration ProjectionTarget
        ↓ dependency
KPI Definition ProjectionTarget
```

## ADA Configuration Manager CURRENT

La composition no incluye Users.

Módulos directos:

```text
Navigation
Tools
KPI Configuration    optional
KPI Definition       optional
```

Users Administration futura no debe reintroducirse como falso `ManagerModule` de
Source/Projection.

La autorización Manager actual mantiene bypass stale de `is_local`/`administrator`;
es un gap separado y no un contrato de Navigation/Profiles.

## Reglas congeladas

```text
LEGACY                      REMOVE
ADAPTERS / SHIMS / ALIASES FORBIDDEN
DOBLE CONTRATO              FORBIDDEN
OLD SCHEMA READERS          FORBIDDEN IN CURRENT RUNTIME
revision -> ProjectionTarget reconstruction REMOVE
expected_source_revision    REMOVE
private projection revision identity REMOVE

GLOBAL USERS
NO APP-SPECIFIC STATE

USERS LOGIN
READ ONLY AGAINST PROMOTED STORE

USERS REGISTRY
DURABLE + VERSIONED BY PROVIDER CONCURRENCY TOKEN, NOT SOURCE RELEASE

PROFILES
GENERIC ATLANTICUS FIRST-CLASS CAPABILITY

NAVIGATION DURABLE AUTHORIZATION
PROFILE KEYS

NAVIGATION -> USERS
FORBIDDEN

NAVIGATION -> ADA ACCESS
FORBIDDEN

NAVIGATION CONFIGURATION -> PROFILES CORE
CURRENT
```

No reabrir Manager core, Source/Projection core ni los dominios Configuration
cerrados para acomodar otro lifecycle.
