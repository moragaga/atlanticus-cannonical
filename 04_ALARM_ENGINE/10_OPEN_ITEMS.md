# Alarm Engine — Open Items

Estado: **CURRENT — RUNTIME DATA-INTEGRATION MIGRATION ADDED AS BLOCKER**

## BLOCKED — Runtime Operational Data integration

El pipeline legacy de Operational Data fue retirado.

`alarms-runtime` todavía depende de ese contrato y no es ejecutable hasta migrar.

Target conceptual:

```text
Alarm evaluator/input contract
    ↓
DataInputSpec
    ↓
DataInputPlanner
    ↓
DataInputLoader
    ↓
DataInputContext
```

La forma exacta del contrato Alarm permanece PLANNED; debe diseñarse en un incremento propio.

No reintroducir legacy para desbloquearlo.

## Historical CLOSED baseline

Antes del cutover de Operational Data se verificó localmente:

```text
READY exact pair
Runtime EFFECTIVE adoption
Runtime CURRENT v1
Runtime FACTS v2
Modeler current projection baseline
per-Tool operator_pool/operator_view
Delivery from Modeler current head
Tool-key Cosmos connection registry
fixed alarm-live-projection container
local physical E2E through Cosmos read-back
```

Es evidencia histórica, no qualification del Runtime CURRENT post-cutover.

## OPEN — Modeler scheduler

```text
CAROUSEL full scheduler
QUEUE_IN_QUEUE full scheduler
rotation window
QIQ fairness
durable ModelerState/checkpoint
recovery
artifact A -> B state migration
stale/disconnection policy
```

## OPEN — presentation/data

```text
effective dynamic cause
Management projections
History/Analytics projections
```

## OPEN — operations/infrastructure

```text
Alarm Runtime data-input migration
production qualification producer
resource provisioning/startup gates
Azure/Docker qualification
physical Engine extraction
Python 3.14.7/Trixie migration
```

## Project NEXT fuera de Alarm

```text
ATLANTICUS-DISTRIBUTION-AND-TOOLING-FINAL-QUALIFICATION
```
