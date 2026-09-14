# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA.

## Planos principales

### Platform

Capacidades transversales:
- backend;
- connectivity;
- integrations;
- web.

`backend/` representa backend jobs y capacidades propias de esos jobs.

`web/` es frontera de primer nivel para:
- Flask/Dash;
- JavaScript/CSS;
- composición Web;
- server-side Python cuya responsabilidad es Web;
- capabilities Web reutilizables.

Connectivity es dual-use y no adquiere ownership funcional.

### Configuration / Administration

Manager administra configuración, authoring, validation, publication, history y projection actions.

Source genérico pertenece a:

```text
web/capabilities/source/
```

Projection genérica exact-release pertenece a:

```text
web/capabilities/projection/core
```

### Operational Data

Operational Data conserva ownership separado para sources, producers, processes, planner y materialization.

### ADA Runtime

ADA Generic compone la experiencia operacional y consume capacidades Atlanticus.

ADA-specific authorization puede consumir/extender contratos genéricos, pero no convertirse en dependencia del core Atlanticus.

## Manager vs ADA Generic

```text
Manager      = administrar configuración
ADA Generic  = consumir configuración y materializar experiencia operacional
```

No comparten ownership de shell/header.

## Configuration vs Data

```text
CONFIGURATION DETERMINES EXISTENCE
DATA DETERMINES STATE
```

## Source vs Projection

Source y Projection son responsabilidades separadas.

```text
Source     = Local | Blob
Projection = Local | Cosmos
```

Projection representa un `SourceReleaseRef` concreto.

Source current nunca se determina desde Cosmos.

## Web Capability Composition

Las capabilities mantienen ownership separado y dependencias explícitas.

### Profiles / Users

Arquitectura CURRENT:

```text
Profiles
   ↑
   │ one-way dependency
Users
```

Contratos:
- Profiles vive en `web/capabilities/profiles/core`;
- Profiles no depende de Users;
- Profiles no depende de ADA;
- Users puede consumir Profiles;
- cross-capability binding pertenece a composición/adapters;
- `atlanticus.web.users.profiles` no existe como namespace productivo;
- no existe shim del namespace anterior.

### Profiles semántico

Profiles core modela exclusivamente Profiles funcionales explícitos.

```text
ProfileCatalog()
→ empty
```

No posee semántica especial de:
- Root;
- Guest/Pending;
- Local;
- John/Jane.

Administrator es un Profile funcional ordinario.

### Profiles durable configuration

`ProfilesConfiguration` es el contrato durable Profiles-owned actual.

```text
ProfilesConfiguration
└── profiles: tuple[ProfileDefinition, ...]
```

Propiedades:
- sólo Profiles funcionales explícitos;
- serialización provider-neutral;
- reconstruye `ProfileCatalog` sin defaults implícitos;
- no depende de Users;
- no implica `profiles.runtime`;
- no implica Source/Projection independiente de Profiles.

### Users durable configuration

`UsersConfiguration` es el contrato durable Users-owned actual.

```text
UsersConfiguration
└── users: tuple[UserConfiguration, ...]
```

Posee invariantes exclusivamente Users:
- ids únicos;
- emails no nulos únicos;
- identidades únicas.

No posee el catálogo de Profiles.

### Composition contract

La referencia Users→Profiles se valida en una frontera que ve ambos contratos:

```text
UsersConfiguration
        +
ProfilesConfiguration
        ↓
UsersProfilesConfiguration
```

`UsersProfilesConfiguration`:
- exige Administrator explícito;
- rechaza `guest` y `local` como Profiles funcionales;
- exige que todo `UserConfiguration.profile_key` resuelva en Profiles;
- aplica la regla también a Users disabled;
- expone `ProfileCatalog` desde el contrato Profiles-owned.

Esta composición no transfiere ownership de Profiles a Users.

## Canonical Users Source

UCS-1 conserva una única exact Source release y separa resources por ownership:

```text
SourceReleaseRef
├── users/configuration.json.gz
└── profiles/configuration.json.gz
```

No existe un segundo coordinator.

No existe un reloj/release current independiente de Profiles.

Escritura nueva:
- Users schema `2`;
- Profiles resource schema `1`;
- `published_by` permanece en el resource Users como metadata funcional del flujo actual.

Lectura histórica:
- Users source schema `1` permanece soportado;
- el aggregate legacy se normaliza a `UsersConfiguration + ProfilesConfiguration`;
- no se vuelve a escribir schema `1`.

## Canonical Users Projection

Payload CURRENT:

```text
ProjectionRecord[UsersProfilesConfiguration]
```

Cosmos Projection schema CURRENT:

```text
schema_version = 2
```

El store:
- lee schema `1` histórico y lo normaliza;
- escribe sólo schema `2`;
- conserva exact release provenance;
- conserva create-only/CAS;
- conserva idempotencia same-target;
- rechaza same exact release con payload diferente.

La compatibilidad durable v1 es reader compatibility, no un shim `SourceReleaseId <-> str`.

## Legacy administrative boundary

`UsersConfigurationCatalog` permanece como aggregate del camino administrativo legacy existente.

Eso no define el ownership canónico nuevo.

El authoring/admin actual todavía debe migrarse a los contratos separados en un incremento independiente.

## Pending / Guest

Pending pertenece a Users:

```text
PendingUserRecord
→ EffectiveUser(
     pending=True,
     profile=None,
     enabled=True,
     is_local=False
  )
```

Pending no depende de `ProfileCatalog`.

Guest:
- no es Profile runtime;
- no es Profile funcional configurable en `ProfilesConfiguration`;
- no aparece como durable field en la escritura Source canónica nueva.

## Root bootstrap

Root pertenece a Identity/bootstrap:

```text
AuthenticatedIdentity
        ↓
BootstrapRootAccessResolver
        ├─ exact issuer + subject_id + enabled
        │      ↓
        │   READY
        │   bootstrap_root=True
        │   user_id=None
        │
        └─ otherwise
               ↓
          fallback AccessResolver
```

Root no se materializa como:
- User;
- Profile;
- nuevo `AccessStatus`.

El bootstrap Root genérico no equivale a política de autorización funcional ADA.

## Local development identities

Local/John/Jane quedan fuera de Profiles.

Su representación runtime final permanece una frontera posterior.

## Service Registry ownership

Una capability sólo registra servicios que le pertenecen.

CURRENT:

```text
create_users_module(runtime)
→ USERS_RUNTIME_SERVICE_KEY
```

Users no publica un ProfileCatalog mediante un service key propio.

## Storage Resource Topology

Cadena vigente:

```text
Capability resource declaration
        ↓
StorageResourceContract[TTopology]
        ↓
resolve_storage_plan(...)
        ↓
ResolvedStorageResource[TTopology]
        ↓
provider bridge
        ↓
Connectivity primitive
        ↓
provision / validate
```

Storage Topology:
- no contiene secretos;
- no construye SDK clients;
- no hace I/O;
- resuelve conflicts/bindings antes del provider.

`users.runtime` permanece el único recurso durable runtime Users confirmado.

No se crea `profiles.runtime` por inferencia.

El resource topology físico de `CosmosUsersConfigurationProjectionStore` continúa OPEN.

## Source / Projection exact-release

Contrato:

```text
ProjectionTarget = SourceKey + SourceReleaseRef
```

`project(target)`:
- lee la release exacta;
- no relee current;
- conserva release identity;
- permite retry del mismo target.

No introducir adaptadores `SourceReleaseId <-> str`.

## Principio de implementación

Definir contratos antes que consumidores.

Backend antes que frontend cuando el contrato pertenece al backend; no usar esta regla para ubicar incorrectamente server-side Web.

Si una solución raíz reemplaza un contrato anterior, hacer cutover limpio:
- sin shims temporales;
- sin re-exports legacy nuevos;
- sin ownership duplicado.

La compatibilidad de lectura de schemas durables históricos no se considera shim temporal cuando es necesaria para replay exact-release.

## Fronteras futuras

No están cerradas por UCS-1:
- admin composition sobre contratos Users/Profiles separados;
- runtime canonical cutover Users;
- exact-release provenance en `users.runtime`;
- Users administrative canonical migration;
- legacy deletion;
- resource topology físico de canonical Users Projection;
- Root physical configuration;
- Local/John/Jane runtime contract.

No crear Profiles Source/Projection independiente sin un requisito nuevo que justifique otro lifecycle/release clock.

## Alarm Engine

Alarm conserva sus fronteras e invariantes cerrados.

No reabrir Alarm para resolver problemas de Users/Profiles/Identity.
