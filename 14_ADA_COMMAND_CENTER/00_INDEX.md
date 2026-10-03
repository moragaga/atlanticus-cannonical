# ADA Command Center — Canonical Index

Estado: **CURRENT — capability parity CLOSED; full Web qualifier BLOCKED separately; Alarm Engine extraction design NEXT**.

Checkpoint:

```text
atlanticus@346e7ac7ba7c21eede8b524613a6adee7e839e55
```

## Current checkpoints

```text
Alarm/Tool shared contracts ownership                   CURRENT
Command Center contracts consumer cutover               IMPLEMENTED
Users/Profiles/Navigation/Manager parity                 CLOSED
Configuration Manager focal qualification               PASS
Generic Application focal qualification                 PASS
Web lock normalization                                  CLOSED
full Web qualifier                                      BLOCKED by Tools contract duplication
production identity                                     OPEN
Alarm Engine physical extraction                        PLANNED / NEXT DESIGN
```

## Current Master domains

```text
Profiles
Navigation
Alarm Configuration
```

Users sigue siendo operación especial snapshot/recovery y puede inyectar `users.replace` cuando el runtime durable configura recovery/catalog. No es un Source Projection domain ordinario.

## Capability parity CURRENT

El wiring legacy queda SUPERSEDED:

```text
UsersAdministrationStore
UserRecord
users_promoted
promoted=
CosmosUsersStore como store administrativo
local duplicate NAVIGATION_SOURCE_KEY
old NavigationPrincipal binding
```

Command Center consume ahora las capabilities genéricas actuales.

## Qualifier blocker separado

`catalog` y `discovery-cosmos` revelan una incompatibilidad upstream de Tools entre `ada.web.tools.*` y `ada.contracts.tools.*`.

No modificar ADA ni introducir adapters en Command Center durante este cierre.

## NEXT único

```text
ADA-ALARM-ENGINE-EXTRACTION-DESIGN
```

El siguiente chat debe partir del backend actual completo como candidato al Engine y limpiar/invertir dependencias hacia Web antes de cualquier movimiento físico.
