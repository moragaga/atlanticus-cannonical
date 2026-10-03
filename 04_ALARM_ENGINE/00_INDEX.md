# Alarm Engine — Index

Estado: **CURRENT — MODELER + DELIVERY BASELINE IMPLEMENTED AND QUALIFIED LOCALLY**

Checkpoint:

```text
atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856
canonical base@8efd59431754059c548ed1e5d1263533b81012cd
```

## Ownership CURRENT

```text
ada-contracts-alarms
    shared Alarm configuration models/snapshots/schemas

ada-command-center/backend/alarms/core
    operational Alarm Engine semantics

ada-command-center/backend/alarms/materialization
    READY runtime/delivery artifacts

ada-command-center/backend/alarms/persistence
    WAL/EFFECTIVE/recovery

processes/alarms-runtime
    operational truth + CURRENT/FACTS

processes/alarms-modeler
    current per-Tool logical projection baseline

processes/alarms-delivery
    Cosmos transport/publication
```

## CURRENT pipeline

```text
Alarm Configuration projection
    ↓
Materialization READY
    ↓
Runtime EFFECTIVE
    ↓
Runtime CURRENT + FACTS
    ↓
Modeler
    ↓
per-Tool AlarmProjectionSnapshot
    ↓
Delivery
    ↓
alarm-live-projection
```

## Important boundary

Modeler CURRENT es un baseline de proyección first-six; no es todavía el scheduler completo de CAROUSEL/QIQ.

## Documents

| Archivo | Rol CURRENT |
|---|---|
| `01_DOMAIN_MODEL.md` | Domain/shared ownership e invariantes de Alarm. |
| `02_RUNTIME_AND_LIFECYCLE.md` | Runtime lifecycle/state. |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL/EFFECTIVE/recovery. |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | leases/fencing. |
| `05_PROJECTION_AND_PUBLICATION.md` | CURRENT/FACTS + Modeler snapshots + Delivery/Cosmos. |
| `06_MANAGEMENT.md` | Management contract, separado del live baseline. |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | snapshot/READY y refinamiento del split. |
| `08_QUALIFICATION_BASELINE.md` | evidencia de qualification actual. |
| `09_DECISION_INDEX.md` | índice histórico de decisiones. |
| `10_OPEN_ITEMS.md` | backlog vigente. |
| `11_SOURCE_LEDGER.md` | provenance del cierre. |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | separación Live/Management/Analytics. |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | exact adoption + frontera Modeler. |
| `14_MODELER_AND_DELIVERY_PIPELINE.md` | pipeline implementado y target scheduler pendiente. |

## NEXT único

```text
Command Center Web consuming alarm-live-projection
```
