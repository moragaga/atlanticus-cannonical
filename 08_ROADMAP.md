# Atlanticus — Roadmap

Estado: **CURRENT ROADMAP — COMMAND CENTER / ALARM BACKEND ANALYSIS NEXT**

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
```

## NEXT único

```text
COMMAND-CENTER-ALARM-BACKEND-ANALYSIS
```

Scope permitido:

```text
inspect ada-command-center CURRENT implementation
inspect Alarm CURRENT backend implementation
compare responsibilities and ownership
identify reusable domain/runtime boundaries
determine whether Alarm should mature into an engine
identify contracts before consumers
identify conflicts against current canonical
produce a concrete recommended architecture
```

No implementar hasta cerrar debate/diseño.

No mezclar con:

```text
KPI backend
KPI operational E2E
Web corrective work
Navigation access
artifacts
.env.detail
distribution
Collector
```

## BLOCKED

```text
KPI-FULL-OPERATIONAL-E2E
```

Depende de correcciones Web previas necesarias para levantar/configurar la aplicación y ejecutar el flujo real.

## PLANNED / SEPARATE

```text
NAVIGATION-PUBLIC-RESTRICTED-CONTRACT
CURRENT-HEAD-ARTIFACT-GENERATION-QUALIFICATION
ENV-DETAIL-FULL-AUDIT
DISTRIBUTION-REGENERATION
ISOLATED-CONSUMER-QUALIFICATION
ADA-GENERIC-REAL-CONFIG-E2E
KPI-FULL-OPERATIONAL-E2E
Collector / Time Status / UI
Python 3.14.7 / Trixie
production Azure / Entra
macOS host sync
```
