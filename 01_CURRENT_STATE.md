# Atlanticus — Current State

Estado: **CURRENT — KPI BACKEND CLOSED; COMMAND CENTER / ALARM ANALYSIS NEXT**

## Autoridad

```text
Implementation
moragaga/atlanticus@2505196019fcc51e5f97ff66a3159beb87fe71f0

Canonical before replacement
moragaga/atlanticus-cannonical@38404e61c69978183cd515be4ca40afed7ef59e8

Decisions
NOT INSPECTED in this closure by explicit instruction
```

## CLOSED / VERIFIED relevante

```text
ADA-TOOL-SCOPED-SOURCE-OWNERSHIP
ADA-USERS-GLOBAL-IDENTITY
ADA-TOOL-USER-MEMBERSHIP
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

## KPI backend CURRENT

```text
KPI Runtime
    ↓
durable evaluation batches
    ↓
KPI Historian
    ├─ durable daily history
    ├─ error history
    ├─ rolling current.parquet
    └─ HistorianAuthority
    ↓
KPI Timeseries Delivery
    ├─ materialized Registry per Tool
    ├─ named Cosmos connections
    ├─ per-Tool checkpoints
    ├─ bounded parallel publication
    └─ schema_version = 2 output
```

`ada-kpis-history` conserva un único package reusable.

Separación CURRENT:

```text
ada.kpis.history.contract
    logical DatasetDefinitions / targets / durable identity

ada.kpis.history.rolling
    logical rolling metadata / grid / horizon / revision invariants

ada.kpis.history.dataset
    shared PyArrow representation and KPI dataset conversion

processes/kpi-historian
processes/kpi-timeseries-delivery
    orchestration only; no direct PyArrow ownership
```

Timeseries usa `DatasetRuntime` como frontera operacional.

`ParquetDatasetStore` se compone debajo de Runtime.

No existe package `ada-kpis-history-tabular`.

## Qualification focal reportada

```text
kpis/history                         31 passed
processes/kpi-historian             45 passed
processes/kpi-timeseries-delivery   28 passed

Ruff check                          PASS
Ruff format --check                 PASS
git diff --check                    PASS
```

Los tres suites se calificaron en procesos pytest separados porque sus directorios de tests usan el mismo namespace top-level `tests.support`.

Ese collision de collection no representa una regresión productiva.

## BLOCKED

```text
KPI-FULL-OPERATIONAL-E2E
```

Razón:

```text
se requieren correcciones Web previas para levantar/configurar la aplicación completa
y ejecutar el flujo real de KPI de extremo a extremo
```

Por lo tanto:

```text
unit/focused KPI qualification = VERIFIED
real integrated KPI runtime E2E = UNVERIFIED / BLOCKED
```

## OPEN / PLANNED separado

```text
Navigation PUBLIC / RESTRICTED
current-head artifact generation
.env.detail complete audit
distribution regeneration
ADA Generic real configuration
macOS host sync / rcssmin
/health/ready functional checks
Python 3.14.7 / Trixie migration
production Azure / Entra validation
```

## NEXT

```text
COMMAND-CENTER-ALARM-BACKEND-ANALYSIS
```

No presupone todavía que Alarmas deba ser un engine independiente.

La maduración a engine debe surgir del análisis del estado real, responsabilidades y fronteras.
