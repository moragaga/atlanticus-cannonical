# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**
Corte de implementación: `moragaga/atlanticus@5b383a3ff4dcbb2cc15f55df4819ebf9e61e63b4`.

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
- suite Web global: 327 passed, 7 skipped;
- Ruff/format de `capabilities/projection/core` GREEN.

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
- Projection Local/Cosmos concreto por dominio: `PLANNED`;
- Manager BASE/SOURCE/WORKSPACE/PROJECTION + history/compare/conflict: `PLANNED`.

Blob parity y recovery ya están validados.

SharePoint + Power Automate siguen destinados a salir del pipeline Source migrado, pero el retiro pertenece al incremento de migración de consumidores.

No borrar aún los adapters Source legacy de Navigation Configuration sin enumerar las rutas exactas y validar que sus consumidores ya migraron.

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
SOURCE-1A.1       CORE + LOCAL          CLOSED / VERIFIED
SOURCE-1A.2       BLOB                  CLOSED / VERIFIED
PROJECTION        EXACT-RELEASE CORE    CLOSED / VERIFIED
MANAGER           BASE/SOURCE/
                  WORKSPACE/PROJECTION  NEXT
```
