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

Las capabilities exactas se declaran de forma independiente en cada `ManagerModule`. Una capability migrada no necesita un lifecycle legacy monolítico.

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

## Exact capability composition en Manager

Manager puede resolver por separado:

```text
DraftValidationWorkflow
ExactSourceReaderWorkflow
ExactSourcePublicationWorkflow
ExactSourceHistoryWorkflow
ExactProjectionWorkflow
```

`workflow_service` legacy sólo es obligatorio para módulos que todavía usan `ConfigurationLifecycleWorkflow`.

No existe fallback silencioso desde una capability exacta declarada hacia un servicio legacy.

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

- Navigation posee definición de rutas;
- Users Activity posee sesiones/page views/active time;
- composition traduce rutas a activity route keys;
- Identity aporta actor, no ownership del historial.

### Composition Users ↔ Manager

```text
Manager                         Users Configuration
   ↑                                   ↑
   └──── web/compositions/users-manager ┘
```

La composition expone bindings explícitos para:

- validation;
- Source read;
- Source publication;
- Source History;
- Projection.

Manager no depende de Users.

Users Configuration no depende de Manager.

## Profiles / Users

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
- no existe shim del namespace anterior `atlanticus.web.users.profiles`.

## Profiles semántico

Profiles core modela exclusivamente Profiles funcionales explícitos.

```text
ProfileCatalog()
→ empty
```

No posee semántica especial de Root, Guest/Pending, Local ni John/Jane.

Administrator es Profile funcional explícito.

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

- local revision identifica payload local;
- base revision identifica BASE local;
- create nace clean;
- edit preserva BASE y exact Source snapshot;
- rebase adopta exact Source snapshot nuevo;
- local revision no es release identity;
- draft schema vigente = `2`;
- no existe parser legacy schema `1` para el draft canónico.

## Manager exact workspace

`ManagerWorkspace` conserva:

- owner;
- payload local;
- exact `SourceSnapshot` como BASE;
- local revision;
- base payload revision.

El exact workspace controller:

- carga Source current por `ExactSourceReaderWorkflow`;
- valida payload por `DraftValidationWorkflow`;
- verifica contra exact Source snapshot;
- publica por `ExactSourcePublicationWorkflow`;
- rebasa sólo después de publicación exitosa.

No usa `source_revision: str` como identidad del Source exacto.

## Exact Projection

Contrato:

```text
ExactProjectionWorkflow
    get_status() -> ProjectionStatus
    get_current_projection_target() -> ProjectionTarget | None
    project(ProjectionTarget) -> ProjectionExecutionResult
```

`ProjectionStatus` es el modelo de `projection/core`:

```text
alignment
source_current_release
projected_source_release
```

Manager no inventa audit actor, projection revision ni timestamps ausentes del core.

## Exact History

Contrato:

```text
ExactSourceHistoryWorkflow
    list_history_exact(limit) -> HistoryPage
    load_history_release_exact(SourceReleaseRef) -> ExactSourceHistoryReadResult
```

Invariantes:

- History lista publicaciones Source, no autosaves;
- `SourceReleaseRef` se conserva completo;
- no se convierte a `RevisionHistoryEntry`;
- no se crea alias textual de release;
- lectura exacta debe devolver la misma identidad solicitada.

History y status son capabilities separadas: fallo/ausencia de History no convierte un status exacto válido en unavailable.

## Historical release -> WORKSPACE

Una release histórica no repunta current.

```text
historical payload
      ↓
local workspace on current BASE
      ↓
dirty local work
      ↓
validate → verify → publish
      ↓
new Source release
```

Si todavía no existe workspace, Manager carga Source current para establecer BASE y después aplica el payload histórico.

## Productive Users host

CURRENT en ADA Configuration Manager:

```text
Users ManagerModule
├── workflow_service = None
├── draft_validation_service
├── exact_source_reader_service
├── exact_source_history_service
├── exact_source_workflow_service
└── exact_projection_service
```

ADA registra las capabilities exactas Users por separado.

Projection llega ya compuesta mediante `ConfigurationManagerDependencies.users_exact_projection`; ADA no extrae SourceStore/ProjectionStore privados.

`UsersManagerWorkflowAdapter` fue retirado.

## Legacy administrative boundary

Legacy permanece sólo donde existan consumidores no migrados.

Para Users Manager:

- authoring activo no usa `UsersConfigurationCatalog`;
- publication no usa lifecycle legacy;
- status no usa `Manager ProjectionStatus` legacy;
- projection no usa adapter legacy;
- History preview no usa catálogo legacy.

No crear adapters exact→legacy para preservar APIs no publicadas.

Navigation/Tools/KPI/KPI Definitions siguen teniendo adapters legacy que deben alinearse en un frente separado.

## Pending / Guest / Root

Pending pertenece a Users.

Guest no es Profile runtime ni Profile funcional configurable.

Root pertenece a Identity/bootstrap y no se materializa como Managed User/Profile.

Local/John/Jane quedan fuera de Profiles; su representación runtime final sigue OPEN.

## Storage Resource Topology

`users.runtime` permanece el único recurso durable runtime Users confirmado.

No se crea `profiles.runtime` por inferencia.

El resource topology físico del canonical Users Projection store continúa OPEN.

## Principio de implementación

Definir contratos antes que consumidores.

Backend antes que frontend cuando el contrato pertenece al backend.

Si una solución raíz reemplaza un contrato anterior:

- cutover limpio;
- sin shims temporales;
- sin re-exports legacy nuevos;
- sin ownership duplicado.

Read/import compatibility histórica durable puede permanecer cuando está explícitamente separada del authoring canónico.

## Fronteras futuras

OPEN / PLANNED:

- ADA legacy Projection contract alignment;
- wiring físico externo y Docker E2E;
- Profile replacement UX;
- Users runtime canonical cutover;
- exact-release provenance en `users.runtime`;
- eliminación legacy;
- resource topology físico canonical Users Projection;
- Root physical configuration;
- Local/John/Jane runtime contract;
- Python 3.14.7 global migration.

No crear Profiles Source/Projection independiente sin requisito nuevo de lifecycle/release propio.
