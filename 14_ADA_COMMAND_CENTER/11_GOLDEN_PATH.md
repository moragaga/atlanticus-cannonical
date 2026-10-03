# ADA Command Center — Golden Path

Estado: **PARTIALLY IMPLEMENTED — BACKEND LIVE ALARM PATH CLOSED LOCALLY; WEB LIVE RENDER NEXT**

Checkpoint:

```text
moragaga/atlanticus:main@38379979fad90e2c514a2d56f3aa3889ceb71856
```

## Current golden path

| Etapa | Estado |
|---|---|
| Tool/Alarm authoring contracts | CURRENT |
| Alarm Configuration projection `alarm-configuration` | CURRENT |
| Qualification mechanism | CURRENT local/manual; production producer OPEN |
| Materialization READY exact pair | CURRENT |
| WAL adoption / EFFECTIVE | CURRENT |
| Runtime CURRENT v1 + FACTS v2 | CURRENT |
| Modeler per-Tool projection | CURRENT |
| Delivery current modeled head | CURRENT |
| Cosmos `alarm-live-projection` | CURRENT / VERIFIED local |
| Command Center Web operational live read/render | NEXT / NOT YET ACCREDITED |
| Management | PLANNED |
| History/Analytics | PLANNED |
| Azure/Entra production qualification | UNVERIFIED |

## Frozen chain

```text
Tool/Alarm configuration
→ AlarmConfigurationSnapshot
→ alarm-configuration
→ qualification
→ READY
→ EFFECTIVE
→ Runtime CURRENT + FACTS
→ Modeler index + per-Tool snapshot
→ Delivery
→ alarm-live-projection
→ Web [NEXT]
```

Exact pin:

```text
source_key + result_id + manifest_sha256 + resolution_key
```

Runtime/Modeler/Delivery no mezclan artifacts.

## Live Web rule

Web debe usar `operator_view` y `alarms` del live snapshot. No procesa WAL, no recalcula prioridad, no reconstruye `operator_pool` y no decide CAROUSEL/QIQ por su cuenta.

## Qualification actual

El backend live path fue probado localmente hasta Cosmos read-back.

No se acredita todavía:

```text
Web live rendering
full scheduler
multi-container Docker
Azure
production qualification producer
Management
History/Analytics
```

## NEXT único

```text
ADA-COMMAND-CENTER-ALARM-LIVE-WEB-CONSUMER
```
