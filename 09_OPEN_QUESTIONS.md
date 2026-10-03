# Atlanticus — Open Questions

Estado: **CURRENT — ALARM LIVE BACKEND BASELINE CLOSED; WEB CONSUMER NEXT**

## CLOSED

```text
Materialization READY real
Runtime EFFECTIVE real
Runtime CURRENT + FACTS real
Runtime -> Modeler baseline
Modeler per-Tool projection baseline
Modeler -> Delivery baseline
Delivery -> Cosmos alarm-live-projection
Cosmos read-back
fixed live container name
Tool-key connection registry
```

## OPEN / NEXT — Web consumer

El próximo chat debe resolver exclusivamente:

```text
1. Qué componente Web actual debe leer alarm-live-projection.
2. Cómo resuelve el Tool/current destination sin reintroducir discovery en Alarm Engine.
3. Cómo mapear operator_view slots y alarms al UI existente.
4. Qué refresh/read policy necesita Web.
5. Cómo representar vacío/stale/error sin recalcular lógica de Runtime/Modeler.
6. Qué tests funcionales prueban lectura/render sin congelar CSS.
```

## OPEN / SEPARATE — Alarm Engine

```text
CAROUSEL/QIQ scheduler completo
90 vs 120 segundos
fairness QIQ
ModelerState durable/checkpoint
artifact A -> B scheduler migration
stale/disconnection policy
resolved effective cause
production qualification producer
physical Engine extraction
```

## OPEN / SEPARATE — Product/Platform

```text
Tool contract duplication blocker
production Entra/Azure
Docker multi-process qualification
resource preparation/startup gates
History/Analytics
Management
artifact completeness/installability
Python migration
```
