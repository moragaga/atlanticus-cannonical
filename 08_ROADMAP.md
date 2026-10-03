# Atlanticus — Roadmap

Estado: **CURRENT ROADMAP — ADA ALARM ENGINE EXTRACTION DESIGN NEXT**

## CLOSED / CURRENT

```text
ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE
ADA-CONSUMER-REPOSITORY-RUNTIME
ADA-TOOL-SCOPED-SOURCE-OWNERSHIP
ADA-USERS-IDENTITY-MEMBERSHIP-CUTOVER
ADA-USERS-RUNTIME
ADA-USERS-RECOVERY-SNAPSHOT
MASTER-PROJECTION-USERS-REPLACE

KPI-NAMED-CONNECTIONS
KPI-REGISTRY-MATERIALIZATION
KPI-LATEST-MULTI-TOOL-DELIVERY
KPI-HISTORIAN-ROLLING-READ-MODEL
KPI-TIMESERIES-MULTI-TOOL-DELIVERY
KPI-HISTORY-DATASET-BOUNDARY

COMMAND-CENTER-USERS-PROFILES-NAVIGATION-MANAGER-PARITY
COMMAND-CENTER-WEB-LOCK-NORMALIZATION
```

## NEXT único

```text
ADA-ALARM-ENGINE-EXTRACTION-DESIGN
```

Scope permitido:

```text
inspect scopes/ada-command-center/backend
inventory all backend packages and dependency directions
treat complete Alarm backend as candidate Engine ownership
identify backend -> Web dependencies
classify KEEP / MOVE / REMOVE / INVERT / REHOME
define Command Center publication -> Engine input contract
decide target scope/package names
freeze dependency graph
```

No mover código hasta cerrar debate/diseño.

## BLOCKED / SEPARATE

```text
COMMAND-CENTER-FULL-WEB-QUALIFIER
```

Causa actual:

```text
Tool contract duplication between ada.web.tools.* and ada.contracts.tools.*
```

No abrir ADA para resolverlo durante Alarm Engine extraction.

## PLANNED / SEPARATE

```text
resolution of ADA Tool contract duplication
current-head artifact generation qualification
.env.detail full audit
distribution regeneration
isolated consumer qualification
ADA Generic real configuration E2E
KPI full operational E2E
Collector / Time Status / UI
Python 3.14.7 / Trixie
production Azure / Entra
macOS host sync
```
