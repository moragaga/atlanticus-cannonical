# ADA Command Center — Canonical Index

Estado: **CURRENT — LIVE ALARM BACKEND VERTICAL IMPLEMENTED; WEB LIVE CONSUMER NEXT**

Checkpoint:

```text
atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856
```

## Current checkpoints

```text
shared Alarm/Tool contracts ownership              CURRENT
Users/Profiles/Navigation/Manager parity           CLOSED
Alarm Configuration authoring/projection           CURRENT
Materialization READY                              CURRENT
Runtime EFFECTIVE + CURRENT + FACTS                CURRENT
Alarm Modeler baseline                             CURRENT
Alarm Delivery -> Cosmos live projection           CURRENT
alarm-live-projection read-back                    VERIFIED local
Web live consumer                                  NEXT
full Web qualifier                                 BLOCKED separately by Tool type duplication
production identity/Azure                         OPEN
Alarm Engine physical extraction                   PLANNED / SEPARATE
```

## Current alarm chain

```text
Command Center Alarm Configuration
    ↓
alarm-configuration projection
    ↓
Materialization
    ↓
Runtime
    ↓
Modeler
    ↓
Delivery
    ↓
alarm-live-projection
    ↓
Command Center Web [NEXT]
```

## NEXT único

```text
ADA-COMMAND-CENTER-ALARM-LIVE-WEB-CONSUMER
```

No mezclar full scheduler, Engine extraction, Management o Analytics en ese incremento.
