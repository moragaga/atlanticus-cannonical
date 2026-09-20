# KPI Backend Recovery / Registry Consumption — Index

Estado: **PLANNED / EXECUTION READY**

Precondición Web:

```text
KPI Registry durable
CLOSED / VERIFIED / CURRENT

KPI Definition durable
CLOSED / VERIFIED / CURRENT
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_PROBLEM.md` | Por qué checkpoints actuales dificultan recovery/tests. | VERIFIED |
| `02_REPROCESS_CONTRACT.md` | Semántica común de reproceso autorizada. | DECIDED |
| `03_KPI_RUNTIME.md` | Reevaluar watermark actual sin dato nuevo. | PLANNED / NEXT |
| `04_LATEST_DELIVERY.md` | Migración de consumer a KPI Registry durable; reprocess separado. | PLANNED |
| `05_HISTORIAN.md` | Reconstruir historia desde durable evaluations. | PLANNED |
| `06_TIMESERIES_DELIVERY.md` | Migración de consumer a KPI Registry durable; reprocess separado. | PLANNED |
| `07_SAFETY_RULES.md` | Gates que jamás se bypassan. | CURRENT |
| `08_CONFIGURATION.md` | `REPROCESS_CURRENT` para jobs autorizados. | DECIDED |
| `09_TESTING.md` | Casos de comportamiento para la secuencia actual. | CURRENT PLAN |
| `10_SOURCE_LEDGER.md` | Código CURRENT inspeccionado. | AUDIT LEDGER |

## Orden de ejecución

```text
1. KPI Runtime reprocess
2. Delivery + Timeseries Registry consumption
3. Historian reprocess
4. ADA Generic Collector
```
