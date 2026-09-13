# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**
Corte de implementación: `moragaga/atlanticus@19eae42574d691a1e3278e4e250849ef9452e9f5`.

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

Navigation implementa una ruta Source canónica:
- `NavigationSourceCodec` serializa una release de configuración como recurso Source;
- `NavigationSourceService` usa `SourceStore`;
- publicación usa `PublishRequest`, `ConcurrencyToken` y `basis_release`;
- History se obtiene desde `SourceStore.query_history`;
- lectura histórica usa `SourceReleaseRef` exacto;
- dos publicaciones pueden compartir contenido/hash y conservar identidades de release distintas;
- `NavigationProjectionBuilder` construye el payload de dominio desde la release exacta.

Navigation implementa stores concretos:
- `LocalNavigationProjectionStore`;
- `CosmosNavigationProjectionStore`.

Ambos implementan `ProjectionStore[NavigationConfigurationCatalog]` y conservan un active projection por `SourceKey`.

El runtime Navigation migrado consume directamente:

```text
ProjectionStore[NavigationConfigurationCatalog]
+ SourceKey
```

y ya no depende del `NavigationProjectionRepository` legacy.

Gates del último incremento en workspace real:
- runtime Navigation: 7/7;
- Navigation Configuration: 49/49;
- Ruff GREEN;
- format GREEN;
- `git diff --check` GREEN;
- suite Web global GREEN con 7 skips conocidos.

La administración Navigation todavía depende del contrato Manager productivo legacy basado en `source_revision: str`.
`NavigationManagerWorkflowAdapter` consume `NavigationConfigurationServices.administration` y `.projection_workflow`.

Por esta razón no se eliminan todavía:
- `NavigationConfigurationSource`;
- `NavigationConfigurationPublisher`;
- `NavigationProjectionRepository`;
- `NavigationConfigurationSourceDocument`;
- `NavigationAdministrationService`;
- `NavigationProjectionWorkflow`;
- adapters Source/Projection legacy.

No introducir shim `SourceReleaseId <-> str` ni un segundo coordinator Manager paralelo sólo para completar esta migración.

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
El cutover de raíz debe reemplazar esa semántica cuando los consumidores necesarios estén preparados; no crear compatibilidad paralela temporal.

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

El repo actual aún conserva 3.14.2 en varios proyectos, incluido Web Source y Projection Core.

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
- Projection Local/Cosmos concreto para Navigation: `IMPLEMENTED + VALIDATED`;
- otros providers Projection concretos por dominio: `PLANNED`;
- Manager BASE/SOURCE/WORKSPACE/PROJECTION contracts: `IMPLEMENTED`;
- Manager productive workflow/callback cutover: `BLOCKED / IN PROGRESS`.

Blob parity y recovery ya están validados.

SharePoint + Power Automate siguen destinados a salir del pipeline Source migrado, pero el retiro pertenece al incremento de migración de consumidores.

No borrar aún los adapters Source legacy de Navigation Configuration: su consumidor administrativo Manager todavía no ha migrado.

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

Dirección decidida para el siguiente frente:

```text
Profiles MUST NOT require Access.
Access MAY consume/extend Profiles.
```

Profiles pertenece a Atlanticus y debe poder instalarse y operar sin Access.

Access es específico de ADA y puede consumir/extender Profiles.
La dependencia `Profiles -> ADA Access` está prohibida.

La implementación actual y la decisión histórica
`Atlanticus_ADA_Usuarios_Perfiles_Acceso_Arquitectura_2026-09-10.docx`
deben auditarse antes de congelar el contrato físico final.

Estado:
`DECIDED DIRECTION / NOT YET AUDITED`.

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

Objetivo:
Web agrega todos los requirements instalados y prepara/valida recursos antes de Backend.

Local:
- puede crear database;
- crea/valida containers.

Cloud:
- database preexistente;
- Web no crea DB;
- crea/valida containers permitidos;
- mismatch de partition/TTL = error contractual.

### Deployment

Orden:

```text
base infra
→ Web
→ resource preparation
→ projection bootstrap
→ Web/Manager READY
→ Backend
```

La Web puede estar disponible sin datos ni backend.

### Manager access

El bypass `is_local → full access` existe actualmente y debe retirarse.

Se introduce una superficie pre-Manager que no depende de Users/Profile projection y permite diagnosticar/preparar infraestructura y proyecciones con autorización de bootstrap independiente.

## KPI Backend recovery

Los cuatro procesos auditados poseen gates `current` que evitan repetir trabajo:
- KPI Runtime;
- Latest Delivery;
- Historian;
- Timeseries Delivery.

Esto es correcto en producción pero dificulta repair/testing cuando se borra un materializado.

Dirección:

```text
REPROCESS_CURRENT
```

por proceso, default false.

El modo sólo omite el shortcut `current`.

No omite:
- authority ordering;
- lease;
- fencing;
- cancellation;
- durable conflicts.

Historian requiere full rebuild desde durable evaluation batches hasta KPI committed watermark.

## Bootstrap closure status

Canonical Baseline 1.0 es suficiente para ejecución.

Primer delivery order:

```text
Operaciones Integradas
→ Mina
```

El foco está en productización vertical, no en expansión arquitectónica general.

## Checkpoint Source / Projection

```text
SOURCE-1A.1                 CORE + LOCAL                     CLOSED / VERIFIED
SOURCE-1A.2                 BLOB                             CLOSED / VERIFIED
PROJECTION                  EXACT-RELEASE CORE               CLOSED / VERIFIED
NAV-SOURCE-PROJECTION-1     SOURCE CONTRACTS                 CLOSED / VERIFIED
NAV-SOURCE-PROJECTION-2     LOCAL/COSMOS PROJECTION STORES   CLOSED / VERIFIED
NAV-CONSUMER-MIGRATION-A    RUNTIME                          CLOSED / VERIFIED
NAV-CONSUMER-MIGRATION-B    ADMIN / MANAGER                  BLOCKED
MANAGER                     ROOT CONTRACT CUTOVER            BLOCKED / IN PROGRESS
NEXT                         USERS-PROFILES-BOUNDARY          PLANNED
```
