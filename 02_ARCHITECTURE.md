# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico no depende de ADA.

## Planos principales

### Platform

Capacidades transversales:
- backend;
- connectivity;
- integrations;
- web.

`backend/` significa backend jobs y capacidades propias de esos jobs.

No significa “todo Python server-side”.

`web/` es una frontera de primer nivel y contiene:
- Flask/Dash;
- JavaScript/CSS;
- composición Web;
- server-side Python cuya responsabilidad es Web;
- capabilities Web reutilizables.

`connectivity/` es dual-use:
- puede ser consumido por Web;
- puede ser consumido por backend/jobs;
- expone conectividad técnica;
- no adquiere ownership funcional del consumidor.

### Configuration / Administration

Manager:
- aplicación administrativa;
- header/shell propio;
- registry;
- workflow;
- authoring;
- validation;
- Source publication;
- history/conflict.

Source genérico pertenece a Web:

```text
web/capabilities/source/
```

Connectivity Storage puede ser dependencia técnica de un provider Blob, pero no es owner de Source.

### Operational Data
- sources;
- producers;
- processes;
- planner;
- materialization.

### ADA Runtime

ADA Generic:
- compone la estructura operacional;
- consume configuración/proyecciones;
- monta runtime Web;
- integra KPI/Alarm/estado operacional.

### Alarm Engine

Core + Persistence + Runtime Process.

No reabrir sus invariantes cerradas para resolver problemas de integración.

## Frontera Manager vs ADA Generic

Manager:
`administrar la configuración`

ADA Generic:
`consumir configuración y materializar la experiencia operacional`

Headers y navegación tienen ownership distinto.

## Tool / Component

Tool Configuration determina estructura.

Component:
- unidad funcional;
- Store;
- Collector contract;
- KPI destination;
- alarm baseline.

Subcomponent:
- unidad visual;
- sin Store/Collector propio;
- target visual de alarma.

## Configuration vs Data

`CONFIGURATION DETERMINES EXISTENCE`

`DATA DETERMINES STATE`

## Source vs Projection

Source y Projection son responsabilidades independientes.

Estado objetivo:

```text
Source     = Local | Blob
Projection = Local | Cosmos
```

Source publica un `SourceReleaseRef` concreto.

Projection representa un Source release específico y puede quedar retrasada o fallar sin revertir Source.

El current de Source nunca se determina desde Cosmos.

## Source ownership

Source es una capability Web genérica.

No pertenece a:
- backend jobs;
- Connectivity;
- ADA;
- Navigation Configuration.

Navigation Configuration y otros dominios pueden consumir Source sin convertirse en owners de su persistencia/versionado.

## Principio de implementación

Definir primero el contrato en el owner correcto y después sus consumidores.

Cerrar capacidades verticalmente sin convertir “backend first” en una regla de ubicación física incorrecta.

Ejemplo para Configuration:

```text
Tool Configuration semantics
→ Web Source
→ Projection
→ runtime consumers
→ E2E
```

Para contratos propios de backend jobs, backend se cierra antes de su frontend consumidor.

No terminar capas horizontales aisladas y recién integrarlas al final.

## ADA Command Center

Es una aplicación hermana de ADA Generic y Manager.

```text
Tool Configuration
     ↓
confirmed topology
     ↓
Command Center Alarm Configuration
     ↓
B.2 Resolution
     ↓
Alarm Engine
     ├── Live → ADA Generic
     └── History/Analytics → Command Center Web
```

Command Center no es owner de Tool Configuration.

Alarm Engine sí pertenece funcionalmente a Command Center.

## Web Capability Composition

Las capacidades Web conservan ownership separado y dependencias explícitas.

El cierre `PROFILES-DOMAIN-EXTRACTION` establece físicamente:

```text
Profiles
   ↑
   │ one-way dependency
Users

Navigation       User Activity
    │                 │
    └──── optional composition ────┐
                                   │
Users / Profiles ──────────────────┘
```

Contratos vigentes:
- Profiles vive en `web/capabilities/profiles/core`;
- Profiles no depende de Users;
- Users core depende de Profiles;
- Users Configuration declara Profiles como dependencia directa cuando consume sus contratos;
- `atlanticus.web.users.profiles` ya no existe como namespace Python productivo;
- no existe shim/re-export de compatibilidad para el namespace eliminado;
- cross-capability binding pertenece a composición/adapters y no justifica dependencias inversas.

La extracción física no congela todavía la semántica final de perfiles base ni la separación Source/Projection de Users y Profiles.

ADA Access puede consumir/extender Profiles mediante composición ADA, pero Profiles no depende de ADA Access.

El dashboard puede agregar capabilities como consumidor/read model sin convertir producers en dependencias mutuas.

## Web Storage Resource Topology

Los requisitos de recursos físicos de una capability Web se declaran en una capa neutral bajo Web; no convierten Connectivity en owner funcional.

La cadena arquitectónica vigente es:

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
Connectivity provider primitive
        ↓
provision / validate
```

Responsabilidades:
- la capability declara qué recurso necesita y sus invariantes;
- composición enlaza conexiones permitidas;
- Storage Topology resuelve conflictos y bindings sin I/O;
- el bridge traduce una topology resuelta al primitive técnico del provider;
- Connectivity crea/valida el recurso físico.

`StorageResourceContract` no contiene secretos, SDK clients ni I/O.

V1 admite overrides genéricos únicamente para:
- `connection_ref`;
- `physical_name` cuando la capability lo autoriza explícitamente.

Owner, provider y topology no son overrides de composición en V1.

Para Cosmos, Web usa `CosmosContainerTopology` con:
- `partition_key_path`;
- `default_ttl_seconds`.

El container name no pertenece a `CosmosContainerTopology`; permanece en el contrato genérico como `default_physical_name`.

`CosmosContainerSpec` permanece en Connectivity como primitive físico del provider. El bridge entre ambos contratos es una responsabilidad separada y no introduce Azure SDK ni secretos en las capabilities Web.

Users confirma el primer recurso de este modelo:

```text
users.runtime
→ cosmos
→ users-runtime
→ partition /id
→ TTL None
```

Pending y Managed comparten el mismo recurso durable.

## Web as Startup Orchestrator

La Web posee el lifecycle de preparación de aplicación:

```text
Web
→ resource plan
→ resource provisioning/validation
→ projection plan
→ readiness
```

Backend declara requisitos neutrales y después opera independientemente.

No:

```text
Backend job
→ Web runtime
```

ni:

```text
cada job iteration
→ ensure infrastructure
```

## Availability vs Readiness

```text
Web shell available
≠
domain ready
```

La Web puede representar READY / DEGRADED / ERROR sin desaparecer ante ausencia de datos/backend.

## Deployment Direction

```text
Base infrastructure
→ Web
→ Application resources/projections
→ Backend
```
