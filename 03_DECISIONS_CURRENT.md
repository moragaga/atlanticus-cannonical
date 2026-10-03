# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global — FROZEN

```text
uv; no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
one focus per increment
Git read-only unless explicit authorization
```

## Command Center capability parity — CURRENT / CLOSED

Command Center consume las capabilities genéricas actuales para Users / Profiles / Navigation / Manager.

El bloqueo Web restante por duplicación de contratos Tool es separado y no debe contaminar el frente Alarm Engine.

## Alarm Engine ownership — CURRENT decision / physical extraction PLANNED

CURRENT físico:

```text
scopes/ada-command-center/backend
```

Target:

```text
scopes/ada-alarm-engine
```

Dirección congelada:

```text
Command Center
    authoring
    semantic validation
    Tool/reference validation/resolution
    routing/visual validation
    publication
        ↓
shared Alarm publication contract
        ↓
Alarm Engine
    core
    materialization
    persistence
    runtime
    modeler
    delivery
```

El target Engine no debe depender de Command Center Web.

## Clean boundary — FROZEN

No crear adapters ni importar módulos externos sólo para leer unos pocos atributos equivalentes.

Regla:

```text
si Alarm sólo transporta un dato
    usar primitive/documented value

si Alarm toma decisiones de dominio sobre el dato
    usar Alarm-owned semantic type
```

Target prohibido:

```text
Engine -> ada-command-center.web.*
Engine -> atlanticus.web.*
Engine -> Tool Catalog discovery
Engine -> ToolStructure runtime dependency
Engine -> mirrored adapters of external Tool models
```

La equivalencia/provenance de valores primitivos externos puede documentarse sin crear dependencia Python de runtime.

## Residual domain — REFINED / final implementation still OPEN

Dirección acordada:

```text
next_routing_tool_kind()
    Command Center pre-publication validation

ALARM_CONFIGURATION_SOURCE_KEY
    shared/primitive publication identity;
    no paquete completo sólo para importar un string
```

La eliminación/rehome física de `scopes/ada-command-center/domain/alarms` sigue PLANNED hasta inventario final de imports.

## Materialization target — REFINED / DESIGN FROZEN

SUPERSEDED como target:

```text
Materialization
    ├── RuntimeAlarmConfiguration
    └── DeliveryAlarmConfiguration
```

Target actual:

```text
Materialization
    ├── RuntimeConfiguration
    ├── ModelerConfiguration
    └── DeliveryConfiguration
```

Los tres pertenecen al mismo artifact pin exacto.

El actual `DeliveryAlarmConfiguration` mezcla modelado y entrega; debe dividirse en el próximo incremento antes de implementar nuevos consumidores.

## Runtime — CURRENT ownership

Runtime posee:

```text
evaluation
occurrence / episode
operational lifecycle
priority semantics
management / deactivation effects
assignments
EFFECTIVE adoption
durable operational facts
```

Runtime no decide slots de pantalla, carousel, queue-in-queue ni transporte Cosmos.

## Modeler — NEW CURRENT decision / implementation PLANNED

Modeler es backend lógico stateful del Alarm Engine.

Posee:

```text
logical positions / slots
ordering
rotation
timers de exposición
queue state
carousel state
queue-in-queue state
reconciliation ante cambios abruptos
durable checkpoint/state
modeled heads consumibles por Delivery
```

No es una capa Web.

No posee CSS, Dash callbacks ni geometría física/pixel layout.

## Delivery — REFINED

CURRENT implementado:

```text
Delivery input receiver consume Runtime CURRENT/FACTS
```

Target:

```text
Delivery consume modeled heads
y se ocupa de transporte/publicación
```

El consumo directo Runtime → Delivery es **SUPERSEDED como target**, pero permanece CURRENT implementado hasta cutover real.

Delivery no debe decidir ordering, positions, dwell time, carousel ni queue-in-queue.

## Backpressure / queue semantics — FROZEN

Runtime → Modeler:

```text
ordered
durable
no-drop para cambios contractualmente necesarios
Runtime no espera acknowledgement de Modeler
Modeler mantiene checkpoint propio
memoria acotada
```

Modeler → Delivery:

```text
durable current head por destination
latest-wins
checkpoint independiente por destination
un destino lento no bloquea otros destinos
```

## CAROUSEL — DESIGN FROZEN parcial

Siempre seis posiciones físicas.

### 0..1 DISTRIBUTED

```text
un único carousel de seis posiciones
```

Una única alarma DISTRIBUTED participa como cualquier otro elemento elegible.

### 2+ DISTRIBUTED

```text
positions 1..5
    carousel normal

position 6
    carousel DISTRIBUTED independiente
```

Las dos rotaciones son independientes.

Las posiciones de cada región visible se compactan de izquierda a derecha sin huecos.

Cuando el conjunto DISTRIBUTED vuelve a menos de dos, se vuelve al único carousel de seis posiciones mediante reconciliación; no resetear estado arbitrariamente si puede preservarse de forma válida.

La duración concreta `90/120 s` está OPEN.

## QUEUE_IN_QUEUE — DESIGN FROZEN parcial

```text
MINE
    4 components
    3 posiciones visibles totales

PLANT
    5 components
    3 posiciones visibles totales

total visible máximo = 6
```

Cada component puede tener candidatos ocultos adicionales.

MINE y PLANT tienen schedulers independientes.

Permanece OPEN la política exacta de fairness entre la cola interna de un component y candidatos todavía no mostrados de otros components.

La duración concreta `90/120 s` está OPEN.

## Reconciliation / recovery — FROZEN

El Modeler se diseña como reconciliador, no como una secuencia rígida de movimientos de índices.

Verdad principal:

```text
Alarm identity + eligibility + scheduler state
```

Derivación:

```text
current logical slots / modeled head
```

Ante desaparición, gestión o pérdida de elegibilidad:

```text
operational truth wins
reconcile immediately
```

Ante crash/restart:

```text
load ModelerState
replay after checkpoint
reconcile(now)
publish present valid head
```

No reproducir obligatoriamente rotaciones visuales históricas vencidas.

## Configuration adoption with live model state — OPEN

Una transición:

```text
artifact A backlog/state
→ artifact B
```

debe definir qué scheduler state se preserva, migra, reconcilia o reinicializa.

No resolver por inferencia dentro del consumidor.

## Exact handoff schemas — OPEN

La semántica está congelada, pero no el documento físico exacto de:

```text
Runtime -> Modeler
Modeler -> Delivery
```

CURRENT v1 + FACTS v2 son superficies implementadas y reutilizables; todavía debe decidirse si el Modeler las consume mediante un lector coordinado o si se publica un contrato explícito adicional por ciclo.

## Next único

```text
ADA-ALARM-ENGINE-MATERIALIZATION-CONTRACT-SPLIT
```
