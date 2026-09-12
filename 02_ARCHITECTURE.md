# Atlanticus — Architecture

Estado: **CANDIDATE V2**

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
- web capabilities.

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

Objetivo:
- Source = Local | Blob;
- Projection = Local | Cosmos.

Projection siempre representa un Source release específico.

## Principio de implementación

Cerrar capacidades verticalmente:

backend contract
→ authority/source
→ materialization/projection
→ KPI/Alarm
→ Web
→ E2E

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

Las capacidades Web permanecen independientes.

```text
Users/Profile       Navigation       User Activity
     │                  │                 │
     └──────── optional composition ──────┘
```

No introducir dependencia funcional directa sólo porque una aplicación use ambas.

El dashboard puede agregarlas como consumidor/read model.

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
