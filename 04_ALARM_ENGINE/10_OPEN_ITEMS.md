# Alarm Engine — Open Items

Estado: **CURRENT — LIVE BASELINE CLOSED; ADVANCED MODELING/OPERATIONS OPEN**

## CLOSED / CURRENT

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

## OPEN — Modeler scheduler

```text
CAROUSEL full scheduler
QUEUE_IN_QUEUE full scheduler
90 vs 120 second rotation window
QIQ fairness
independent scheduler timers
durable ModelerState/checkpoint
recovery of scheduler state
artifact A -> B state migration
stale/disconnection policy
```

Current first-six `operator_view` is not evidence that these are implemented.

## OPEN — Runtime → Modeler evolution

CURRENT baseline uses Runtime CURRENT v1.

If future scheduler/history semantics require every transition, define the durable ordered no-drop handoff explicitly. Do not assume current head alone satisfies that future requirement.

## OPEN — presentation/data

```text
effective dynamic cause
Management projections
tracking_view
inactive_reactivation_view
History/Analytics projections
Projection Facts if required
```

## OPEN — operations/infrastructure

```text
production qualification producer
resource provisioning/startup gate for live container
Azure/Docker qualification
physical Engine extraction
Python 3.14.7/Trixie migration
```

## Known minor observability gap

Modeler puede escribir `CURRENT_MODELED` y aun mostrar `work=0/empty=1` porque el job actual no marca iteration work. No afecta el snapshot generado, pero la métrica debe corregirse en un incremento propio si se usa operacionalmente.

## NEXT fuera del Engine core

```text
Command Center Web consumer of alarm-live-projection
```
