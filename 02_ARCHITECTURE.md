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

`web/` es frontera de primer nivel para Flask/Dash, JavaScript/CSS, composición Web, server-side Python con responsabilidad Web y capabilities Web reutilizables.

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

Una composition se justifica cuando:
- capability A debe seguir siendo reusable sin B;
- capability B debe seguir siendo reusable sin A;
- el binding necesita conocer ambas;
- ninguna de las dos debe adquirir ownership de la otra.

No usar `web/compositions` como capa obligatoria ni como cajón general.

### Composition existente: Navigation ↔ User Activity

```text
Navigation
    ↓
navigation-activity
    ↓
ActivityRouteResolver
    ↓
Users Activity
```

Ownership:
- Navigation posee la definición de rutas;
- Users Activity posee sesiones, page views, active time y route activity;
- `navigation-activity` traduce rutas Navigation a `ActivityRoute`/route keys;
- Identity aporta contexto del actor, pero no posee el historial de actividad.

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
- cross-capability validation pertenece a una frontera que ve ambos contratos;
- `atlanticus.web.users.profiles` no existe como namespace productivo;
- no existe shim del namespace anterior.

### Profiles semántico

Profiles core modela exclusivamente Profiles funcionales explícitos.

```text
ProfileCatalog()
→ empty
```

No posee semántica especial de Root, Guest/Pending, Local ni John/Jane.

Administrator es un Profile funcional ordinario.

### Profiles durable configuration

`ProfilesConfiguration` es el contrato durable Profiles-owned actual.

No implica `profiles.runtime` ni Source/Projection independiente de Profiles.

### Users durable configuration

`UsersConfiguration` es el contrato durable Users-owned actual.

Posee invariantes exclusivamente Users:
- ids únicos;
- emails no nulos únicos;
- identidades únicas.

No posee el catálogo de Profiles.

### Composition contract Users + Profiles

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
- aplica la regla también a Users disabled.

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

Cosmos Projection escribe schema `2`, lee schema `1` histórico, conserva exact release provenance y CAS.

## Canonical admin composition

El backend administrativo canónico opera directamente sobre `UsersProfilesConfiguration`.

```text
UsersProfilesAdminDraft
├── owner_subject_id
├── configuration: UsersProfilesConfiguration
├── source_snapshot: SourceSnapshot
├── revision
├── base_payload_revision
└── saved_at_utc
```

Semántica:
- `revision` identifica el payload local actual;
- `base_payload_revision` identifica la BASE local;
- draft recién creado queda limpio;
- `with_configuration(...)` preserva BASE y `SourceSnapshot`;
- `rebase(...)` adopta un nuevo exact `SourceSnapshot` y convierte la revisión actual en nueva BASE;
- local revision no es release identity;
- schema vigente del draft = `2`;
- no existe parser legacy/schema 1 para el draft canónico.

La precondición de publicación es el `SourceSnapshot` exacto.

## Canonical Users Admin Web

CURRENT desde `d23bff...`:

```text
Users admin active path
├── canonical_layout.py
├── canonical_callbacks.py
├── UsersAdminWebContext.administration
│   └── UsersProfilesAdministrationService
├── CATALOG_STORE_ID
│   └── UsersProfilesConfiguration document
└── DRAFT_BASIS_STORE_ID
    └── UsersProfilesAdminDraft schema 2
```

Reglas:
- UI edits operan sobre el payload canónico, no `UsersConfigurationCatalog`;
- browser draft incompatible/schema 1 se descarta y se reconstruye clean desde Source current;
- no existe adapter browser draft legacy→schema 2;
- save local preserva exact `SourceSnapshot` mediante `with_configuration(...)`;
- save local no publica Source;
- layout usa `dcc.Store(memory)`; no declara IndexedDB global implementado;
- import legacy de archivo es compatibilidad explícita de entrada: decode legacy → split canónico → local draft sobre la BASE existente;
- legacy Web files pueden permanecer físicamente hasta cleanup, pero no son el active export/registration path.

Profile delete mantiene el contrato backend de replacement explícito. La UI actual evita borrar un Profile referenciado en vez de implementar todavía la UX de replacement.

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

`ExactSourcePublicationResult` conserva `PublishResult` tipado.

`ManagerProjectionCoordinator` transporta el snapshot exacto y detecta stale source sin reinterpretar strings legacy.

El protocolo:
- convive con `ConfigurationLifecycleWorkflow`;
- no redefine `source_revision: str`;
- no crea segundo coordinator.

## Composition Users ↔ Manager exact-source

CURRENT:

```text
Manager                     Users Configuration
   ↑                               ↑
   └── compositions/users-manager ─┘
```

`UsersManagerExactSourceWorkflow` es el adapter de integración exact-source.

Responsabilidades:
- delegar `get_source_snapshot()`;
- parsear payload con `UsersProfilesConfiguration.from_document(...)`;
- obtener actor mediante `UsersAuditActorProvider`;
- delegar publication exacta al backend Users;
- devolver `ExactSourcePublicationResult`;
- audit timestamp = timestamp de la release publicada.

No posee:
- draft/session;
- `rebase`;
- Projection execution;
- Dash/UI;
- IndexedDB;
- registro de servicios del host.

La composition depende de Manager + Source + Users Configuration.

Manager no adquiere dependencia de Users y Users Configuration no adquiere dependencia de Manager.

## Productive host boundary

El host ADA tiene ahora una frontera partida intencionalmente:

```text
Users Admin Web
    → UsersProfilesAdministrationService
    → payload/draft canónico

Manager Users workflow registration
    → UsersManagerWorkflowAdapter
    → contrato legacy
```

`ConfigurationManagerDependencies` exige `users_profiles_administration` para construir el editor Users canónico.

El registro productivo de publicación Users todavía no usa `UsersManagerExactSourceWorkflow`.

No se identificó dentro de `atlanticus` el constructor productivo externo de `ConfigurationManagerDependencies`; el wiring físico de `users_profiles_administration` permanece UNVERIFIED.

El próximo cutover debe cerrar la publication/service boundary sin adaptar el payload canónico de vuelta a `UsersConfigurationCatalog`.

## Legacy administrative boundary

`UsersConfigurationCatalog` ya no es el contrato del editor Users activo.

Permanece legacy donde todavía hay consumidores, incluyendo el workflow productivo de Manager y history preview asociado.

La compatibilidad durable/import histórica no justifica un adapter de authoring canónico→legacy.

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

Su representación runtime final permanece abierta.

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

`project(target)` lee release exacta, no relee current y conserva release identity.

No introducir adaptadores `SourceReleaseId <-> str`.

## Principio de implementación

Definir contratos antes que consumidores.

Backend antes que frontend cuando el contrato pertenece al backend.

Si una solución raíz reemplaza un contrato anterior, hacer cutover limpio:
- sin shims temporales;
- sin re-exports legacy nuevos;
- sin ownership duplicado.

La compatibilidad de lectura durable/import histórica necesaria no se considera shim temporal cuando permanece explícitamente separada del authoring canónico.

## Fronteras futuras

No están cerradas todavía:
- cutover productivo del workflow Users hacia exact-source en Manager;
- verificación del wiring físico externo de `users_profiles_administration`;
- UX de replacement para Profile referenciado;
- runtime canonical cutover Users;
- exact-release provenance en `users.runtime`;
- eliminación legacy;
- resource topology físico de canonical Users Projection;
- Root physical configuration;
- Local/John/Jane runtime contract;
- Python 3.14.7 global migration.

No crear Profiles Source/Projection independiente sin requisito nuevo de lifecycle/release propio.

## Alarm Engine

Alarm conserva sus fronteras e invariantes cerrados.

No reabrir Alarm para resolver Users/Profiles/Identity.
