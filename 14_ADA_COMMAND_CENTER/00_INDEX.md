# ADA Command Center — Canonical Index

Estado: **CURRENT implementation at `6725237...`; ada-contracts cutover IN PROGRESS qualification; capability parity BLOCKED/NEXT**.

## Current checkpoints

```text
Alarm/Tool shared contracts ownership        CURRENT
Command Center contracts consumer cutover    IMPLEMENTED
cutover qualification                        IN PROGRESS
configuration-manager                        BLOCKED by capability drift
Users/Profiles/Navigation parity with ADA    PLANNED / NEXT
production identity                          OPEN
final distribution gate                      PLANNED after parity
```

## Current Master domains

```text
Profiles
Navigation
Alarm Configuration
```

Users sigue siendo operación especial de runtime/recovery cuando se habilita, no un Source Projection domain normal.

## Current implementation drift

Command Center todavía contiene wiring anterior para Users y parte de Navigation:

```text
UsersAdministrationStore
UserRecord
CosmosUsersStore as administration
promoted=
local NAVIGATION_SOURCE_KEY declaration
older NavigationPrincipal binding
```

Las capacidades genéricas CURRENT y ADA ya usan el modelo nuevo.

## NEXT único

```text
COMMAND-CENTER-CAPABILITY-PARITY
Users + Profiles + Navigation + Manager
```

Objetivo: llevar Command Center al mismo nivel arquitectónico que ADA reutilizando las capabilities genéricas, no copiando lógica específica de ADA.

## AFTER NEXT

```text
resume ada-contracts qualifier
→ full GREEN gate
→ final dependency/diff audit
→ artifact/distribution work
```
