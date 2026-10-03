# Alarm Engine — Index

Estado: **CURRENT — shared contracts cutover implemented; Command Center parity closed; physical Engine extraction design NEXT**.

Checkpoint:

```text
atlanticus@346e7ac7ba7c21eede8b524613a6adee7e839e55
```

## Ownership CURRENT

```text
ada-contracts-tools
    shared Tool contracts

ada-contracts-alarms
    shared Alarm contracts + Engine publication schemas

ada-command-center/domain/alarms
    residual Command Center/Alarm policy pending reclassification

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

Working hypothesis for the next chat:

```text
all of the current Alarm backend belongs to the Engine;
the main extraction debt is dependency direction toward Web.
```

This is PROPOSED / PLANNED, not yet physically implemented.

## Qualification state

Capability parity no longer blocks qualification.

Focal passes:

```text
configuration-manager   31 passed
generic-application     12 passed
catalog-manager          8 passed
```

The resumed Web qualifier is BLOCKED separately at Tool Catalog / Discovery because current ADA Tool configuration still produces `ada.web.tools.*` types while Command Center catalog consumes `ada.contracts.tools.*`.

Do not solve that by changing Alarm contracts or adding Command Center adapters.

## Documents

| Archivo | Rol actual |
|---|---|
| `01_DOMAIN_MODEL.md` | Alarm model contracts. |
| `02_RUNTIME_AND_LIFECYCLE.md` | lifecycle and Engine state. |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL/EFFECTIVE/recovery. |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | concurrency and fencing. |
| `05_PROJECTION_AND_PUBLICATION.md` | CURRENT/FACTS. |
| `06_MANAGEMENT.md` | management/deactivation. |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | publication/materialization boundary. |
| `08_QUALIFICATION_BASELINE.md` | historical qualification evidence. |
| `09_DECISION_INDEX.md` | current canonical decisions/refinements. |
| `10_OPEN_ITEMS.md` | next extraction boundary. |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Command Center ↔ Engine ↔ Web/Analytics boundary. |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | exact adoption/runtime contracts. |

## NEXT único

```text
ADA-ALARM-ENGINE-EXTRACTION-DESIGN
```

First inventory and freeze dependencies. No code move until the design is closed.
