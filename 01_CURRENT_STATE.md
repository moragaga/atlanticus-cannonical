# Atlanticus — Current State

Estado: **CURRENT — ALARM ENGINE EXTRACTION BOUNDARY REFINED; MODELER TARGET DESIGN FROZEN; IMPLEMENTATION NEXT**

## Autoridad

```text
Implementation HEAD inspected
moragaga/atlanticus@09e9acf6edf6f84a66a4a0a041ad9a8f645daf79

Alarm implementation unchanged since
moragaga/atlanticus@346e7ac7ba7c21eede8b524613a6adee7e839e55

Canonical source before these replacements
moragaga/atlanticus-cannonical@f02b4740ca1002b060afdb94d142f2e2d8d588af
```

El compare `346e7ac7...09e9acf6` contiene cuatro commits y ningún cambio bajo las rutas Alarm del Command Center. Por tanto el inventario/lectura de Alarm realizado contra `main` continúa representando la implementación relevante de este frente.

## CLOSED / VERIFIED previo relevante

```text
COMMAND-CENTER-USERS-PROFILES-NAVIGATION-MANAGER-PARITY
COMMAND-CENTER-WEB-LOCK-NORMALIZATION

Alarm shared contracts
Alarm Core lifecycle/priority/persistence
READY != EFFECTIVE
exact artifact adoption
Engine CURRENT v1
Engine FACTS v2
current direct Delivery input receiver
```

## Alarm backend CURRENT físico

Continúa bajo:

```text
scopes/ada-command-center/backend/
```

Paquetes candidatos del Engine:

```text
alarms/core
alarms/materialization
alarms/persistence
processes/alarms-materialization
processes/alarms-runtime
processes/alarms-delivery
```

El target físico sigue siendo extracción limpia hacia `scopes/ada-alarm-engine`; no se implementó durante este hito.

## Materialization CURRENT implementado

El artefacto materializado actual contiene pareja:

```text
RuntimeAlarmConfiguration
DeliveryAlarmConfiguration
```

`DeliveryAlarmConfiguration` contiene información que hoy mezcla responsabilidades de modelado/proyección y entrega, incluyendo:

```text
Alarm identity + display metadata
messages
visual_targets
    tool_key
    tool_kind
    component_keys
    subcomponents
    process_projection_mode
```

`ProcessAlarmProjectionMode` actual define:

```text
GENERIC
DISTRIBUTED
```

## Runtime publication CURRENT implementado

Runtime publica:

```text
runtime/output/current/latest.json
runtime/output/facts/facts-<hash>.json
runtime/output/state/facts-export-cursor.json
```

CURRENT v1 contiene estado operacional completo de occurrences abiertas, incluyendo evaluación, prioridad, holds, management/deactivation y assignments.

FACTS v2 contiene lotes durables encadenados por `previous_batch`, `journal_position`, commit/hash y records.

## Delivery input CURRENT implementado

El proceso actual `alarms-delivery` recibe directamente CURRENT/FACTS y mantiene su inbox/cursor propio.

Esta implementación sigue siendo **CURRENT / VERIFIED**.

Como frontera target, el consumo directo Runtime → Delivery queda **SUPERSEDED** por la decisión de diseño de este hito:

```text
Runtime
    ↓
Modeler
    ↓
Delivery
```

No borrar ni reinterpretar la evidencia CURRENT hasta que el nuevo pipeline esté implementado y cualificado.

## Target Engine pipeline — DESIGN FROZEN / IMPLEMENTATION PLANNED

### Configuration plane

```text
Command Center
    authoring
    semantic validation
    Tool/reference resolution
    routing/visual validation
    publication
        ↓
AlarmConfigurationSnapshot
        ↓
Materialization
        ├── RuntimeConfiguration
        ├── ModelerConfiguration
        └── DeliveryConfiguration
```

### Data plane

```text
Runtime
    ↓ durable / ordered / no-drop handoff
Modeler
    ↓ durable latest head per destination
Delivery
    ↓ transport/publication
Projection Store / Cosmos
    ↓
Web
```

## Modeler ownership — DESIGN FROZEN

Modeler es una capa lógica backend stateful del Alarm Engine.

Posee:

```text
logical slots/positions
ordering
rotation timers
queue state
carousel state
queue-in-queue state
change reconciliation
recovery/checkpoint
disconnection/staleness state when contractually defined
modeled projection heads
```

No posee:

```text
CSS
Dash layout
pixel geometry
Web callbacks
Cosmos transport mechanics
Tool Catalog discovery
```

Runtime conserva verdad operacional de alarmas; Modeler deriva el estado lógico consumible; Delivery publica el resultado ya modelado.

## CAROUSEL — DESIGN FROZEN parcial

Siempre existen seis posiciones físicas.

Con `0..1` alarmas `DISTRIBUTED` elegibles:

```text
un solo carousel
capacity = 6
GENERIC y una DISTRIBUTED pueden ocupar las mismas seis posiciones
```

Con `2+` alarmas `DISTRIBUTED` elegibles:

```text
positions 1..5
    scheduler normal

position 6
    scheduler DISTRIBUTED independiente
```

La cola normal y la cola DISTRIBUTED rotan de forma independiente.

Dentro de cada región las posiciones visibles se mantienen compactas sin huecos.

Desaparición, gestión o pérdida de elegibilidad provoca reconciliación inmediata; el timer no obliga a conservar una alarma que ya no es elegible.

La ventana exacta `90 vs 120 segundos` permanece OPEN.

## QUEUE_IN_QUEUE — DESIGN FROZEN parcial

Topología acordada:

```text
MINE
    4 components
    3 posiciones visibles totales

PLANT
    5 components
    3 posiciones visibles totales

total máximo visible = 6
```

Un component puede contener más alarmas elegibles que sus alarmas actualmente visibles; existen candidatos ocultos.

MINE y PLANT evolucionan de forma independiente.

El algoritmo exacto de fairness entre:

```text
alarmas ocultas del mismo component
vs
candidatos de otros components
```

permanece OPEN.

La ventana exacta `90 vs 120 segundos` permanece OPEN.

## Throughput / backpressure — DESIGN FROZEN

```text
Runtime nunca espera al Modeler.
Modeler nunca espera a Delivery para persistir su propio estado.
```

Runtime → Modeler:

```text
durable
ordered
no-drop para los cambios necesarios
checkpoint propiedad del Modeler
bounded reads / bounded memory
```

Modeler → Delivery:

```text
latest-wins por destination/projection head
checkpoint independiente por destino
un destino lento no bloquea los demás
```

El backlog pertenece a almacenamiento durable, no a memoria de los procesos.

## Recovery — DESIGN FROZEN

Modeler debe persistir checkpoint y estado suficiente para:

```text
load durable state
replay sólo cambios posteriores al checkpoint
reconcile against now
rebuild current modeled head
continue
```

No reproducir como frames visibles todas las rotaciones históricas vencidas durante una caída.

Cambios múltiples de un mismo batch/ciclo se reconcilian antes de emitir una nueva revisión modelada; no publicar una secuencia de estados intermedios inútiles durante inicialización masiva.

## BLOCKED separado

```text
COMMAND-CENTER-FULL-WEB-QUALIFIER
```

Sigue separado por coexistencia de tipos Tool:

```text
ada.web.tools.*
ada.contracts.tools.*
```

No resolverlo como parte del Alarm Engine / Modeler.

## NEXT único

```text
ADA-ALARM-ENGINE-MATERIALIZATION-CONTRACT-SPLIT
```

Objetivo:

```text
definir con precisión y luego implementar incrementalmente
la separación del actual DeliveryAlarmConfiguration en:

ModelerConfiguration
DeliveryConfiguration
```

sin implementar todavía CAROUSEL/QUEUE_IN_QUEUE y sin mezclar la extracción física completa.
