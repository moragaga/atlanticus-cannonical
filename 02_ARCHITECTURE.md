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

La administración de un dominio puede exponer un workflow exact-source opt-in sin reemplazar de una vez todos los workflows legacy.

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

No implica `profiles.runtime` ni Source/Projection independiente de Profiles.

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

Una única exact Source release separa resources por ownership:

```text
SourceReleaseRef
├── users/configuration.json.gz
└── profiles/configuration.json.gz
```

No existe segundo coordinator ni reloj independiente de Profiles.

Escritura nueva:
- Users schema `2`;
- Profiles resource schema `1`;
- `published_by` permanece en Users.

Lectura histórica:
- Users source schema `1` permanece soportado;
- aggregate legacy se normaliza;
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
- lee schema `1` histórico y normaliza;
- escribe sólo schema `2`;
- conserva exact release provenance;
- conserva create-only/CAS;
- conserva idempotencia same-target;
- rechaza misma exact release con payload diferente.

## Canonical admin composition

El backend administrativo canónico nuevo opera directamente sobre:

```text
UsersProfilesConfiguration
```

No introduce un `UsersProfilesAdminCatalog` ni otro aggregate durable mixto.

El estado administrativo separa:
- payload editable;
- snapshot Source exacto usado como base;
- revisión local del draft;
- metadata local del draft.

```text
UsersProfilesAdminDraft
├── owner_subject_id
├── configuration: UsersProfilesConfiguration
├── source_snapshot: SourceSnapshot
├── revision
└── saved_at_utc
```

`revision` identifica contenido del draft; no identifica Source release.

La precondición de publicación es el `SourceSnapshot` exacto:
- `SourceKey`;
- current `SourceReleaseRef` cuando existe;
- current content hash informativo;
- `ConcurrencyToken` cuando existe.

El backend de Users pasa:
- `basis_release = source_snapshot.current.release_ref`;
- `expected_concurrency_token = source_snapshot.concurrency_token`.

No convertir esas identidades a `str`.

### Admin mutation rules

Administrator:
- Profile explícito;
- no eliminable;
- key estable;
- edición dedicada de colores en el contrato actual.

Profile funcional:
- key derivada al crear;
- key inmutable al editar;
- delete referenciado requiere replacement explícito;
- reasignación + eliminación ocurre en una única transformación validada.

Managed User:
- creación administrativa parte de Pending;
- identidad autenticada se conserva;
- identidad de un Managed existente no es editable.

## Manager exact-source publication boundary

Manager incorpora un protocolo opt-in:

```text
ExactSourcePublicationWorkflow
├── get_source_snapshot() -> SourceSnapshot
└── publish_draft_exact(
       payload,
       expected_source_snapshot
   ) -> ExactSourcePublicationResult
```

`ExactSourcePublicationResult` contiene el `PublishResult` tipado de Source.

`ManagerProjectionCoordinator` transporta el snapshot exacto y detecta stale source sin reinterpretar strings legacy.

El protocolo exact-source:
- convive con `ConfigurationLifecycleWorkflow`;
- no fuerza migración de módulos legacy;
- no redefine `source_revision: str`;
- no crea segundo coordinator;
- no es todavía el wiring productivo de Users.

## Legacy administrative boundary

`UsersConfigurationCatalog` permanece como aggregate del camino administrativo productivo legacy mientras callbacks/layout/store no hayan migrado.

El nuevo backend canónico existe en paralelo como destino del cutover, pero no debe adaptarse de vuelta al aggregate legacy.

La compatibilidad durable de schema histórico no justifica un adapter de authoring mixto.

## Pending / Guest

Pending pertenece a Users y no depende de `ProfileCatalog`.

Guest:
- no es Profile runtime;
- no es Profile funcional configurable;
- no aparece como durable field en escritura Source nueva.

## Root bootstrap

Root pertenece a Identity/bootstrap y no se materializa como User/Profile.

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

Users no publica ProfileCatalog mediante service key propio.

## Storage Resource Topology

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

Backend antes que frontend cuando el contrato pertenece al backend.

Si una solución raíz reemplaza un contrato anterior, hacer cutover limpio:
- sin shims temporales;
- sin re-exports legacy nuevos;
- sin ownership duplicado.

La compatibilidad de lectura durable histórica necesaria para replay exact-release no se considera shim temporal.

## Fronteras futuras

No están cerradas todavía:
- callbacks/layout/browser draft cutover de Users admin;
- wiring Users ↔ Manager exact-source;
- runtime canonical cutover Users;
- exact-release provenance en `users.runtime`;
- eliminación legacy;
- resource topology físico de canonical Users Projection;
- Root physical configuration;
- Local/John/Jane runtime contract.

No crear Profiles Source/Projection independiente sin requisito nuevo de lifecycle/release propio.

## Alarm Engine

Alarm conserva sus fronteras e invariantes cerrados.

No reabrir Alarm para resolver Users/Profiles/Identity.
