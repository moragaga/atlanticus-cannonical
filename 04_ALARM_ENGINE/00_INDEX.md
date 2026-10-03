# Alarm Engine — Index

Estado: **CURRENT — extraction boundary refined; Modeler target design frozen; implementation next**.

Checkpoints:

```text
Repository HEAD inspected
atlanticus@09e9acf6edf6f84a66a4a0a041ad9a8f645daf79

Alarm implementation unchanged since
atlanticus@346e7ac7ba7c21eede8b524613a6adee7e839e55

Canonical source before these replacements
atlanticus-cannonical@f02b4740ca1002b060afdb94d142f2e2d8d588af
```

## Ownership CURRENT

```text
ada-contracts-tools
    shared Tool contracts

ada-contracts-alarms
    shared Alarm contracts + Engine publication schemas

ada-command-center/domain/alarms
    residual source/routing policy pending physical reclassification

ada-command-center/backend
    current physical Alarm Engine implementation
```

## Backend candidate Engine

Current physical packages:

```text
alarms/core
alarms/materialization
alarms/persistence
processes/alarms-materialization
processes/alarms-runtime
processes/alarms-delivery
```

Target logical Engine now includes a new responsibility:

```text
modeler
```

Its physical package/process does not exist yet.

## Target pipeline

```text
Command Center
    ↓ publication
Materialization
    ├── RuntimeConfiguration
    ├── ModelerConfiguration
    └── DeliveryConfiguration
        ↓
Runtime
    ↓
Modeler
    ↓
Delivery
    ↓
Projection Store
    ↓
Web
```

Current direct Runtime → Delivery receiving remains implemented but is SUPERSEDED as the target boundary.

## Qualification state

Command Center capability parity is closed.

The resumed full Web qualifier is BLOCKED separately at Tool Catalog / Discovery because ADA Tool configuration still produces `ada.web.tools.*` types while Command Center catalog consumes `ada.contracts.tools.*`.

Do not solve that by changing Alarm contracts or adding adapters.

## Documents

| Archivo | Rol actual |
|---|---|
| `01_DOMAIN_MODEL.md` | Alarm domain model contracts. |
| `02_RUNTIME_AND_LIFECYCLE.md` | lifecycle and Engine state. |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL/EFFECTIVE/recovery. |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | concurrency and fencing. |
| `05_PROJECTION_AND_PUBLICATION.md` | CURRENT/FACTS current implementation + target Runtime/Modeler/Delivery handoffs. |
| `06_MANAGEMENT.md` | management/deactivation. |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | publication/materialization and Runtime/Modeler/Delivery configuration split. |
| `08_QUALIFICATION_BASELINE.md` | historical qualification evidence. |
| `09_DECISION_INDEX.md` | current canonical decisions/refinements. |
| `10_OPEN_ITEMS.md` | remaining open contracts before implementation. |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Command Center ↔ Engine ↔ Modeler ↔ Delivery ↔ Web/Analytics boundary. |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | exact adoption/runtime current implementation and target handoff refinement. |
| `14_MODELER_AND_DELIVERY_PIPELINE.md` | Modeler scheduling, backpressure, recovery and delivery target contract. |

## NEXT único

```text
ADA-ALARM-ENGINE-MATERIALIZATION-CONTRACT-SPLIT
```

Definir y luego implementar sólo la separación de configuración antes de construir el Modeler operacional.
