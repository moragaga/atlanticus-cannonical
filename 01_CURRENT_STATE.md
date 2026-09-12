# Atlanticus — Current State

Estado: **CANDIDATE V2**
Corte de implementación: `moragaga/atlanticus@685924322c9cc0d625d112e25297a407f7a46acb`.

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

Connectivity:
- cosmos;
- docker;
- http-client;
- key-vault;
- redis;
- service-bus;
- sql;
- storage.

Operational Data:
- calendar;
- core;
- planner;
- processes;
- producers;
- sources.

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

El repo auditado aún conserva 3.14.2 en varios proyectos.

Estado:
`DECIDED / NOT YET IMPLEMENTED GLOBALLY`.

### Configuration Source
Dirección aprobada:
- Productivo: Azure Blob Storage.
- Local: provider equivalente.
- Projection: Cosmos DB / Local.

Estado:
`DECIDED DIRECTION / CONTRACT NOT FROZEN / IMPLEMENTATION NOT STARTED`.

SharePoint + Power Automate quedan destinados a salir del pipeline Source migrado sólo después de paridad/recovery.

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

En `main` auditado existe sólo backend físico:
- Alarm Core;
- Alarm Persistence;
- Alarms Runtime.

Web propia: `DECIDED/EXPECTED, NOT PRESENT IN AUDITED MAIN`.

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

Los packages base de Users, Navigation y User Activity ya están separados en `main`.

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

Cosmos ya posee `CosmosProvisioner`.

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

Canonical Baseline 1.0 is sufficient to begin execution.

First delivery order:

```text
Operaciones Integradas
→ Mina
```

The focus now moves from architecture expansion to vertical productization.
