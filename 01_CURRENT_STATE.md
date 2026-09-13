# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**
Corte de implementación: `moragaga/atlanticus@139ee93a118e51f66c3d585f00235f212a2475c1`.

## Estado implementado

### Plataforma genérica

Backend transversal:
- configuration;
- datasets;
- datasets-parquet;
- datasets-runtime;
- json;
- kernel;
- observability;
- observability-azure;
- runtime.

`backend/` representa backend jobs y capacidades asociadas a esos jobs.

La lógica Python server-side de aplicaciones Web pertenece a `web/` cuando su responsabilidad es Web.

Connectivity:
- cosmos;
- docker;
- http-client;
- key-vault;
- redis;
- service-bus;
- sql;
- storage.

Connectivity es reutilizable por Web y backend/jobs.

No es owner funcional de Source, Manager, Tool Configuration ni otras capacidades Web.

Operational Data:
- calendar;
- core;
- planner;
- processes;
- producers;
- sources.

### Web Storage Topology / Users Storage Topology

Storage Topology está implementado como capability Web genérica en:

```text
web/capabilities/storage/topology
```

Package:

```text
atlanticus-web-storage-topology==0.1.0
```

Estado:

```text
WEB-STORAGE-TOPOLOGY    CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY  CLOSED / VERIFIED / CURRENT
```

El contrato genérico implementa:
- `StorageResourceContract[TTopology]`;
- `StorageResourceOverride`;
- `ResolvedStorageResource[TTopology]`;
- `ResolvedStoragePlan`;
- `StorageResourceOverrideField`;
- `resolve_storage_plan(...)`.

El resolver es puro y provider-neutral:
- no contiene secretos;
- no contiene clientes provider;
- no usa Azure SDK;
- no realiza I/O;
- deduplica declaraciones idénticas por `logical_id`;
- rechaza declaraciones incompatibles con el mismo `logical_id`;
- rechaza overrides desconocidos o no permitidos;
- falla si falta un connection binding requerido;
- rechaza dos `logical_id` que resuelvan al mismo `(provider, connection_ref, physical_name)`;
- produce un plan inmutable y determinista.

`CosmosContainerTopology` describe únicamente:
- `partition_key_path`;
- `default_ttl_seconds`.

El nombre físico del container permanece en `StorageResourceContract`; Connectivity conserva la responsabilidad de materializar/validar el provider físico.

Users declara un único recurso durable:

```text
logical_id              users.runtime
owner                   users
provider                cosmos
default_connection_ref  None
default_physical_name   users-runtime
partition_key_path      /id
default_ttl_seconds     None
```

`users.runtime` requiere que composición entregue `connection_ref`.
Users permite override de `connection_ref`, pero no de `physical_name`, owner, provider ni topology.

Pending y Managed comparten este recurso. No se crean `users.pending`, `users.managed`, `users.projection` ni `profiles.runtime` por defecto.

Invariante durable:

```text
Users durable data MUST NOT be automatically deleted by Cosmos TTL.
```

Gates ejecutados en el workspace real:
- 19 tests focalizados de Web Storage Topology GREEN;
- 39 tests focalizados Storage Topology + Users Storage GREEN;
- suite Web global GREEN con 7 skips conocidos;
- Ruff GREEN;
- format GREEN;
- `uv lock` GREEN;
- imports públicos GREEN;
- `git diff --check` GREEN.

El intento previo de ubicar este contrato en `backend/storage-topology` fue descartado antes de integrarse y queda `SUPERSEDED`.

### Cosmos Storage Preflight Bridge

Implementado en:

```text
web/capabilities/storage/cosmos
```

Package:

```text
atlanticus-web-storage-cosmos==0.1.0
```

Estado:

```text
STORAGE-PREFLIGHT-COSMOS-BRIDGE  CLOSED / VERIFIED / CURRENT
```

API pública:
- `to_cosmos_container_spec(...)`;
- `ensure_cosmos_storage_plan(...)`;
- `validate_cosmos_storage_plan(...)`.

Contrato vigente:
- traduce recursos Cosmos del `ResolvedStoragePlan` a `CosmosContainerSpec`;
- recibe `Mapping[str, CosmosProvisioner]` preconstruido por composición;
- no recibe secretos, endpoints ni raw settings;
- no construye `CosmosClient`;
- no importa Azure SDK;
- no llama `ensure_database()`;
- resuelve topology, specs, conflictos y provisioners antes del primer provider I/O;
- soporta múltiples conexiones nombradas por `connection_ref`;
- ignora providers no Cosmos;
- un plan sin recursos Cosmos es no-op;
- mismatch físico continúa siendo responsabilidad explícita de Connectivity y nunca se corrige silenciosamente.

Qualification del checkpoint del bridge:
- 17 tests focalizados GREEN;
- Storage Topology + bridge: 51 GREEN;
- Storage Topology + bridge + Users core: 87 GREEN;
- suite Web del checkpoint: 448 passed, 7 skipped;
- Ruff, format focalizado, imports públicos, `uv lock` y `git diff --check` GREEN.

### Users Cosmos Runtime Adapter

Implementado en:

```text
web/capabilities/users/cosmos
```

Package:

```text
atlanticus-web-users-cosmos==0.1.0
```

Estado:

```text
COSMOS-USERS-RUNTIME-ADAPTER  CLOSED / VERIFIED / CURRENT
```

`CosmosUsersRuntimeStore` implementa simultáneamente:
- `UsersRuntimeStore`;
- `PendingUsersReader`.

Contrato durable implementado:

```text
id == partition key == user_id
user_id = build_user_key(issuer, subject_id)
record_type = pending | resolved
```

Semántica:
- `resolve()` usa point-read;
- `observe()` usa create-only y, ante `CosmosConflictError`, vuelve a leer el estado durable vigente;
- `observe()` no usa upsert y por tanto no puede sobrescribir una promoción concurrente;
- `list_pending()` consulta Pending cross-partition y devuelve orden determinista por `user_id`;
- documento corrupto o identidad incompatible falla explícitamente y nunca se degrada silenciosamente a Guest;
- errores Cosmos se traducen a `UsersRuntimeStoreUnavailableError` conservando causa;
- el nombre físico del container se inyecta desde composición;
- el adapter no provisiona database ni containers;
- Users core continúa provider-neutral.

Qualification final en workspace real:
- package Users Cosmos: 23 passed;
- Storage Topology + Storage Cosmos + Users core + Users Cosmos: 110 passed;
- suite Web global: 471 passed, 7 skipped;
- Ruff check GREEN;
- Ruff format GREEN;
- contrato público `CosmosUsersRuntimeStore -> UsersRuntimeStore + PendingUsersReader` GREEN;
- `git diff --check` GREEN.

### Users Runtime Projection Boundary

Implementado en:

```text
web/capabilities/users/configuration
web/capabilities/users/projection-cosmos
```

Packages actuales:

```text
atlanticus-web-users-configuration==0.1.9
atlanticus-web-users-projection-cosmos==0.1.1
```

Estado:

```text
USERS-RUNTIME-PROJECTION-BOUNDARY  CLOSED / VERIFIED / CURRENT
```

Contrato implementado:
- `UsersRuntimeProjectionWriter` es un contrato snapshot-level separado de `UsersRuntimeStore` y `PendingUsersReader`;
- `UsersRuntimeMaterializingProjectionRepository` materializa runtime antes de avanzar el catálogo/estado de proyección legacy;
- `CosmosUsersRuntimeProjectionWriter` es provider-specific y recibe un cliente Cosmos ya construido + container name;
- el runtime adapter de lectura/observación no adquiere responsabilidades administrativas.

Semántica durable Managed:
- writer posee `resolved + is_local=false`;
- Pending→Resolved conserva `id == partition key == user_id`;
- usuario configurado no observado se crea directamente Resolved;
- usuario removido no se elimina: queda Resolved, `enabled=false`, `managed_state=retired`;
- re-add restaura `managed_state=present` y valores actuales de Source legacy;
- actualización existente usa ETag/CAS y no blind upsert;
- conflicto create con observación concurrente reread/promueve sin sobrescribir silenciosamente;
- replay del mismo snapshot converge semánticamente;
- fallo parcial de runtime no avanza el estado global de proyección legacy;
- Local Resolved queda fuera del ownership del writer Managed.

Provenance legacy implementado por documento:
- `projection_source_revision`;
- `projected_by`;
- `projected_at_utc`.

`projection_source_revision` continúa recibiendo `UsersConfigurationBundle.revision`, un digest de contenido legacy. No equivale a `SourceReleaseId`.

Además, `UsersAccessResolver` rechaza un Managed deshabilitado antes de requerir su perfil histórico.

Qualification del checkpoint:
- tests focalizados Users core + Configuration runtime projection + Projection Cosmos: 33 passed;
- suite Web: 496 passed, 7 skipped;
- Ruff check GREEN;
- Ruff format GREEN;
- `uv lock` GREEN;
- `git diff --check` GREEN.

Checkpoint de implementación:
`moragaga/atlanticus@4758d993296bfe2a629a9aa3b8e4b486cf7b2305`.

### Web Source y Projection Handoff

Source implementado en:

```text
web/capabilities/source/
├── core
├── local
└── blob
```

Projection Core implementado en:

```text
web/capabilities/projection/core
```

Packages:

```text
atlanticus-web-source==0.1.0
atlanticus-web-source-local==0.1.0
atlanticus-web-source-blob==0.1.0
atlanticus-web-projection==0.1.0
```

Estado:

```text
Source Core          VERIFIED / CURRENT
Local Source         VERIFIED / CURRENT
Blob Source          VERIFIED / CURRENT
Projection Handoff   VERIFIED / CURRENT
```

Core Source implementa contratos neutrales para:
- releases inmutables;
- current manifest;
- contenido e integridad;
- publicación con concurrencia optimista;
- history;
- lectura de releases;
- verificación de integridad.

Local implementa semántica durable equivalente:
- manifest como único commit point;
- releases inmutables;
- CAS real entre procesos;
- candidatos perdedores como orphans;
- history sólo por cadena de publicaciones;
- recovery por reinicio;
- integridad verificable.

Blob implementa la misma semántica funcional sobre Azure Blob Storage:
- `StorageClient` inyectado; Source no usa Azure SDK directamente;
- manifest como único commit point;
- create-only para first publish;
- conditional write por ETag para promociones posteriores;
- `ConcurrencyToken` público derivado del manifest e independiente del ETag;
- releases inmutables y orphans fuera de History;
- History sólo por predecessor chain;
- recovery explícito ante ACK ambiguo;
- integridad y restart verificados.

Projection Core implementa el handoff exact-release:
- target explícito `SourceKey + SourceReleaseRef`;
- `project(target)` resuelve la release exacta con `read_release`;
- la ejecución no vuelve a consultar Source current;
- provenance durable con `source_release_id`;
- retry del mismo target sin republish de Source;
- alignment `NEVER_PROJECTED / CURRENT / OUTDATED`;
- outcome de intento `SUCCESS / FAILED`;
- `CURRENT / OUTDATED` se determina por identidad de release, no por content hash.

Los gates Source Core + Local + Blob quedaron GREEN en el workspace real.
El provider Blob tiene pruebas deterministas y 7 pruebas de integración Azurite GREEN, incluyendo carreras de first publish/update, ETag real, corrupción y recovery de ACK ambiguo.

Projection Handoff quedó GREEN en el workspace real:
- 15 tests de Projection;
- suite Web global del checkpoint Projection: 327 passed, 7 skipped;
- Ruff/format de `capabilities/projection/core` GREEN.

### Users Configuration — Canonical Source

Implementado en:

```text
web/capabilities/users/configuration
```

Package actual:

```text
atlanticus-web-users-configuration==0.1.9
```

Estado:

```text
USERS-CANONICAL-SOURCE-1  CLOSED / VERIFIED / CURRENT
```

Users expone una ruta Source canónica:
- `UsersSourceCodec` serializa `UsersConfigurationCatalog + published_by` como un recurso Source;
- resource path: `users/configuration.json.gz`;
- document type: `atlanticus_users_configuration_release`;
- schema de dominio Source: `1`;
- JSON compacto y gzip determinista;
- `UsersSourceRelease` combina payload de dominio con `SourceReleaseMetadata`;
- `UsersSourceService` usa `SourceStore`;
- publicación usa `PublishRequest`, `ConcurrencyToken` y `basis_release`;
- History se obtiene desde `SourceStore.query_history`;
- `load_current()` selecciona current una vez y luego lee esa `SourceReleaseRef` exacta;
- `load_release()` valida que Source devuelva el mismo `SourceKey` y `SourceReleaseRef` solicitado;
- dos publicaciones pueden compartir contenido/hash y conservar identidades de release distintas.

Users no reimplementa:
- release identity;
- History;
- CAS/concurrency;
- provider Local/Blob.

Qualification del checkpoint Source:
- tests dirigidos Source Users: 7 passed;
- Users Configuration: 54 passed;
- suite Web global: 503 passed, 7 skipped;
- Ruff check GREEN;
- Ruff format GREEN;
- `uv lock` GREEN;
- `git diff --check` GREEN.

Checkpoint:
`moragaga/atlanticus@f996905c353de26c42bc4907e32a1f2f0c161648`.

### Users Configuration — Canonical Projection

Implementado en:

```text
web/capabilities/users/configuration
web/capabilities/users/projection-cosmos
```

Packages:

```text
atlanticus-web-users-configuration==0.1.9
atlanticus-web-users-projection-cosmos==0.1.1
```

Estado:

```text
USERS-CANONICAL-PROJECTION-2  CLOSED / VERIFIED / CURRENT
```

Contrato implementado:
- `UsersProjectionBuilder` transforma una Source release exacta en `UsersConfigurationCatalog`;
- `create_users_projection_service(...)` reutiliza `SourceProjectionService`;
- target = `SourceKey + SourceReleaseRef`;
- `project(target)` resuelve esa release exacta y no consulta Source current durante ejecución;
- same content en releases distintas sigue siendo target distinto;
- una release histórica exacta puede proyectarse y luego quedar `OUTDATED`;
- `UsersConfigurationBundle.revision` no cruza la frontera canónica;
- el actor de publicación no forma parte del payload proyectado.

Provider Cosmos:
- `CosmosUsersConfigurationProjectionStore` implementa `ProjectionStore[UsersConfigurationCatalog]`;
- first active write create-only;
- replace con ETag/CAS;
- nunca blind upsert;
- same exact release + same payload converge como retry idempotente;
- same release ID con metadata o payload incompatible falla explícitamente;
- conflicto concurrente same-target converge;
- conflicto concurrente different-target produce `UsersConfigurationProjectionConflictError`;
- el container name se recibe desde composición y no se hardcodea;
- no se infiere ordering por release ID ni timestamps;
- target histórico explícito permitido.

Provenance canónico persistido:
- `source_key`;
- `source_release_id`;
- `source_published_at_utc`;
- `projected_at_utc`.

Qualification final:
- 10 tests nuevos focalizados GREEN;
- Users Configuration + Projection Cosmos: 84 passed;
- suite Web: 513 passed, 7 skipped;
- `uv lock --check` GREEN;
- Ruff check GREEN;
- `git diff --check` GREEN.

Checkpoint:
`moragaga/atlanticus@139ee93a118e51f66c3d585f00235f212a2475c1`.

Los contratos administrativos legacy permanecen porque `UsersManagerWorkflowAdapter` y el Manager productivo todavía trabajan con `source_revision: str`.

El cierre canónico de Projection no modifica el provenance legacy de `users.runtime` ni crea equivalencia entre `UsersConfigurationBundle.revision` y `SourceReleaseId`.

### Navigation Configuration — Source / Projection

Package actual:

```text
atlanticus-web-navigation-configuration==0.1.8
```

Estado:

```text
NAV-SOURCE-PROJECTION-1   Canonical Source backend contracts   CLOSED / VERIFIED / CURRENT
NAV-SOURCE-PROJECTION-2   ProjectionStore Local + Cosmos       CLOSED / VERIFIED / CURRENT
NAV-CONSUMER-MIGRATION-A  Runtime canonical consumer           CLOSED / VERIFIED / CURRENT
NAV-CONSUMER-MIGRATION-B  Administrative consumer              BLOCKED
Navigation legacy delete                                      BLOCKED
```

Navigation implementa:
- `NavigationSourceCodec`;
- `NavigationSourceService`;
- publicación con `ConcurrencyToken` y `basis_release`;
- History desde Source;
- lectura histórica por `SourceReleaseRef`;
- mismo contenido con releases distintas;
- `NavigationProjectionBuilder`;
- `LocalNavigationProjectionStore`;
- `CosmosNavigationProjectionStore`.

El runtime Navigation consume:

```text
ProjectionStore[NavigationConfigurationCatalog]
+ SourceKey
```

y ya no depende del `NavigationProjectionRepository` legacy.

Gates del último incremento:
- runtime Navigation: 7/7;
- Navigation Configuration: 49/49;
- Ruff GREEN;
- format GREEN;
- `git diff --check` GREEN;
- suite Web global GREEN con 7 skips conocidos.

La administración Navigation todavía depende del Manager legacy basado en `source_revision: str`.

No se eliminan todavía:
- `NavigationConfigurationSource`;
- `NavigationConfigurationPublisher`;
- `NavigationProjectionRepository`;
- `NavigationConfigurationSourceDocument`;
- `NavigationAdministrationService`;
- `NavigationProjectionWorkflow`;
- adapters Source/Projection legacy.

No introducir shim `SourceReleaseId <-> str` ni un segundo coordinator Manager paralelo.

### ADA

`scopes/ada/` separa backend y Web.

ADA Generic Application compone actualmente:
- branding;
- navegación ADA;
- operational header ADA;
- alarm management/status;
- content state;
- operational render binding/state;
- runtime experience;
- source participation;
- time status;
- global indicators.

### Manager

`ada-configuration-manager` es aplicación independiente sobre `atlanticus.web.manager`.

Módulos actualmente integrados:
- users;
- navigation;
- tools;
- KPI Configuration opcional;
- KPI Definition opcional.

Manager posee Home/navegación/header administrativo propio.

No confundir con ADA operational header.

Manager ya contiene contratos canónicos iniciales para BASE/SOURCE/WORKSPACE/PROJECTION en `workspace.py`:
- `ManagerWorkspace`;
- `ManagerSourceVerification`;
- `ManagerPublicationContext`;
- `SourceSnapshot`;
- `ConcurrencyToken`;
- `SourceReleaseRef`;
- `ProjectionTarget`.

Estado:

```text
Manager canonical workspace/source contracts   CURRENT
Manager productive coordinator cutover         BLOCKED / IN PROGRESS
Manager IndexedDB workspace persistence        PLANNED
```

El coordinator productivo y sus workflows continúan usando `source_revision: str`.

El cutover de raíz debe reemplazar esa semántica; no crear compatibilidad paralela temporal.

### Alarm Engine

Fronteras físicas:
- alarms/core;
- alarms/persistence;
- processes/alarms-runtime.

Qualification R3.5 final: CLOSED PASS/GREEN.

## Estado objetivo decidido

### Python

- Python 3.14.7.
- `python:3.14.7-slim-trixie`.

El repo actual aún conserva 3.14.2 en varios proyectos, incluidos los packages Users afectados por el último hito.

Estado:
`DECIDED / NOT YET IMPLEMENTED GLOBALLY`.

La migración 3.14.2 → 3.14.7 es un incremento transversal separado y no se mezcla con Source/Projection.

### Configuration Source

Dirección aprobada:
- Productivo: Azure Blob Storage.
- Local: provider equivalente.
- Projection: Cosmos DB / Local por dominio cuando corresponda.

Estado actual:
- Source Core: `IMPLEMENTED + VALIDATED`;
- Local provider: `IMPLEMENTED + VALIDATED`;
- Blob provider: `IMPLEMENTED + VALIDATED`;
- Projection exact-release Core: `IMPLEMENTED + VALIDATED`;
- `source_release_id` en Projection Core: `IMPLEMENTED + VALIDATED`;
- Navigation canonical Source: `IMPLEMENTED + VALIDATED`;
- Users canonical Source: `IMPLEMENTED + VALIDATED`;
- Projection Local/Cosmos concreto para Navigation: `IMPLEMENTED + VALIDATED`;
- Users canonical exact-release Projection: `IMPLEMENTED + VALIDATED`;
- otros providers Projection concretos por dominio: `PLANNED`;
- Manager BASE/SOURCE/WORKSPACE/PROJECTION contracts: `IMPLEMENTED`;
- Manager productive workflow/callback cutover: `BLOCKED / IN PROGRESS`.

Blob parity y recovery ya están validados.

SharePoint + Power Automate siguen destinados a salir del pipeline Source migrado, pero el retiro pertenece al incremento de migración de consumidores.

No borrar aún adapters Source legacy de Navigation ni Users Configuration: sus consumidores administrativos Manager todavía no han migrado.

### Manager browser workspace

Dirección decidida:

```text
dcc.Store(memory)   = estado activo de sesión
IndexedDB           = persistencia browser del WORKSPACE
SourceStore / Blob  = autoridad durable publicada
ProjectionStore     = proyección activa durable
```

IndexedDB:
- no es autoridad;
- no sustituye Source;
- no usa inicialmente gzip/base64;
- debe integrarse mediante JavaScript dedicado + `clientside_callback`;
- perder IndexedDB sólo puede perder trabajo no publicado.

Estado:
`DECIDED / NOT YET IMPLEMENTED`.

### Users / Profiles / Access

Dirección decidida:

```text
Profiles MUST NOT require Access.
Access MAY consume/extend Profiles.
```

Profiles pertenece a Atlanticus y debe poder instalarse y operar sin Access.

Access es específico de ADA y puede consumir/extender Profiles.
La dependencia `Profiles -> ADA Access` está prohibida.

Cierres independientes verificados:

```text
USERS-STORAGE-TOPOLOGY            CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER      CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1          CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2      CLOSED / VERIFIED / CURRENT
```

Estos cierres fijan:
- `users.runtime` y su reader/observe durable Cosmos;
- ownership y semántica del writer Managed snapshot-level;
- transición Pending→Resolved, retirement/re-add y CAS del runtime;
- Source canónico de Users;
- Projection canónica exact-release de Users;
- provider Cosmos de la Projection canónica con CAS e idempotencia same-target.

No congelan todavía:
- provenance exact-release dentro de los documentos Managed de `users.runtime`;
- resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`;
- la frontera completa Profiles / ADA Access;
- el cutover administrativo Manager;
- el borrado legacy.

La implementación restante y la decisión histórica
`Atlanticus_ADA_Usuarios_Perfiles_Acceso_Arquitectura_2026-09-10.docx`
deben auditarse antes de congelar el resto del contrato físico y de composición de Profiles/Access.

Estado de la frontera completa:
`DECIDED DIRECTION / IN PROGRESS / NOT YET FULLY AUDITED`.

Siguiente frontera aislada:

```text
MANAGER-ROOT-CANONICAL-CUTOVER  BLOCKED / IN PROGRESS
```

Debe reemplazar el root productivo basado en `source_revision: str` sin shim release/string ni coordinator paralelo.

### Collector

La semántica de Collector está congelada:
- 1 Collector contract por Component;
- no por Subcomponent.

No existe hoy capability top-level literalmente llamada `collectors`.

Debe mapearse contra Producers/Sources/Processes antes de crear una frontera física nueva.

## Dirección hacia entregable

La prioridad es cerrar una vertical usable:

Configuration
→ Source Release
→ Operational Data / Collector
→ KPI / Alarm
→ ADA Generic
→ E2E

Manager participa como plano administrativo con shell propio; no como shell de la Tool operacional.

## ADA Command Center

En `main` existe sólo backend físico:
- Alarm Core;
- Alarm Persistence;
- Alarms Runtime.

Web propia: `DECIDED/EXPECTED, NOT PRESENT IN CURRENT MAIN`.

Configuration propia: necesaria, sin Tool authoring duplicado.

Consume Tool topology confirmada y configura:
- Rules;
- Messages;
- evaluator parameters;
- deactivation;
- escalation;
- visual targets.

Entra ID + Navigation + Profiles forman parte de la dirección inicial.

User Activity, generic Actions y app/session auto-refresh no se priorizan inicialmente.

La finalidad inicial es análisis histórico profundo y conclusiones trazables sobre todas las alarmas.

## Web Platform / Deployment

### Capability independence

Los packages base de Users, Navigation y User Activity están separados en `main`.

Existe además un precedente correcto:

```text
atlanticus-web-composition-navigation-activity
```

que enlaza capabilities sin acoplar sus cores.

Gap actual:
ADA Manager obtiene Navigation profile options directamente desde Users.

La frontera Users/Profiles debe preservar independencia de Access.
ADA Access puede consumir Profiles mediante composición/extensión ADA, sin convertir Access en dependencia de Atlanticus Profiles.

### User Activity

Estado actual:
- session summary;
- route aggregates;
- active seconds;
- route changes.

Objetivo:
- historia ordenada por visita/página;
- TTL funcional 24 h;
- dashboard reconstruye secuencia sin forzar Navigation como dependency.

### Resource preparation

Cosmos dispone de `CosmosProvisioner`.

La Web dispone de contratos neutrales de resource topology, una declaración durable real (`users.runtime`) y el bridge provider-specific hacia Connectivity Cosmos.

Estado de la cadena:

```text
Storage resource contracts        CLOSED / VERIFIED / CURRENT
Users storage declaration         CLOSED / VERIFIED / CURRENT
Cosmos preflight bridge           CLOSED / VERIFIED / CURRENT
Users Cosmos runtime adapter      CLOSED / VERIFIED / CURRENT
Users runtime projection boundary CLOSED / VERIFIED / CURRENT
Users canonical Source            CLOSED / VERIFIED / CURRENT
Users canonical Projection        CLOSED / VERIFIED / CURRENT
Web lifecycle/resource readiness  OPEN / BLOCKED BY GLOBAL CONTRACTS
```

El bridge existente no implica que Web ya ejecute resource preparation en startup.

`ApplicationResourcePlan`, required/optional semantics, named connection resolution global y READY/DEGRADED/ERROR continúan abiertos.

El resource físico del canonical Users Projection store tampoco quedó congelado por este hito.
