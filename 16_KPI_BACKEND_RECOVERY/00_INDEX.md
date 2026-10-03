# KPI Backend Recovery / Materialization / Delivery — Index

Estado: **CLOSED / VERIFIED LOCALLY / CURRENT**

## Autoridad de este cierre

```text
Implementation
moragaga/atlanticus@2505196019fcc51e5f97ff66a3159beb87fe71f0

Canonical inspected before replacement
moragaga/atlanticus-cannonical@38404e61c69978183cd515be4ca40afed7ef59e8

Decisions
NOT INSPECTED in this closure by explicit instruction
```

Git permanece **SOLO LECTURA**.

## Estado por capability

| Archivo | Estado |
|---|---|
| `01_PROBLEM.md` | CLOSED / historical motivation. |
| `02_REPROCESS_CONTRACT.md` | CURRENT; no reabierto. |
| `03_KPI_RUNTIME.md` | CLOSED / VERIFIED / CURRENT; no reabierto. |
| `04_LATEST_DELIVERY.md` | CLOSED / VERIFIED / CURRENT. |
| `05_HISTORIAN.md` | CLOSED / VERIFIED / CURRENT. |
| `06_TIMESERIES_DELIVERY.md` | CLOSED / VERIFIED / CURRENT; multi-Tool rolling consumer implemented. |
| `07_SAFETY_RULES.md` | CURRENT. |
| `08_CONFIGURATION.md` | CURRENT for Materialization + Latest + Timeseries. |
| `09_TESTING.md` | CURRENT qualification baseline. |
| `10_SOURCE_LEDGER.md` | CURRENT audit ledger. |
| `11_MATERIALIZATION.md` | CLOSED / VERIFIED / CURRENT; consumed by Latest and Timeseries. |

## Checkpoints

```text
KPI-NAMED-CONNECTIONS                    CLOSED / VERIFIED / CURRENT
KPI-REGISTRY-MATERIALIZATION             CLOSED / VERIFIED / CURRENT
KPI-LATEST-MULTI-TOOL-DELIVERY           CLOSED / VERIFIED / CURRENT
KPI-HISTORIAN-ROLLING-READ-MODEL         CLOSED / VERIFIED / CURRENT
KPI-TIMESERIES-MULTI-TOOL-DELIVERY       CLOSED / VERIFIED / CURRENT
KPI-HISTORY-DATASET-BOUNDARY              CLOSED / VERIFIED / CURRENT

KPI-FULL-OPERATIONAL-E2E                  BLOCKED / UNVERIFIED
```

## Shared Historian rolling contract

```text
<application_root>/timeseries/current.parquet
```

```text
maximum physical horizon = 24 h
grid                      = 30 s
shape                     = wide
timestamp                 = UTC
physical coverage         = observed only
authority                 = durable history + HistorianAuthority
```

Physical I/O is owned below `DatasetRuntime`.

`ada.kpis.history.dataset` owns the reusable KPI PyArrow representation.

Historian and Timeseries processes do not own PyArrow conversion logic.

## Timeseries output contract

```text
logical/output step = 120 s
schema_version      = 2
id                  = timeseries
partition_id        = kpis
document_type       = ada_kpi_timeseries_delivery
container           = ada-kpi-timeseries-delivery
```

## Remaining boundary

No internal KPI backend contract remains OPEN in this hito.

Operational E2E remains BLOCKED until the required Web corrections allow the full application/runtime flow to be exercised.
