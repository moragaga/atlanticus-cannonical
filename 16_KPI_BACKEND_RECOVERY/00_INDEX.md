# KPI Backend Reprocessing / Recovery — Index

Estado: **CANDIDATE / CONTRACT DESIGN**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_PROBLEM.md` | Por qué los checkpoints actuales dificultan recuperación/pruebas. | VERIFIED |
| `02_REPROCESS_CONTRACT.md` | Semántica común de reproceso. | PROPOSED |
| `03_KPI_RUNTIME.md` | Reevaluar watermark actual sin dato nuevo. | PROPOSED |
| `04_LATEST_DELIVERY.md` | Republicar snapshot actual. | PROPOSED |
| `05_HISTORIAN.md` | Reconstruir historia desde durable evaluations. | PROPOSED |
| `06_TIMESERIES_DELIVERY.md` | Republicar series desde Historian actual. | PROPOSED |
| `07_SAFETY_RULES.md` | Qué gates jamás se bypassan. | CURRENT DIRECTION |
| `08_CONFIGURATION.md` | Flags/configuración candidata. | PROPOSED |
| `09_TESTING.md` | Casos que deben proteger el contrato. | PROPOSED |
| `10_SOURCE_LEDGER.md` | Código actual inspeccionado. | AUDIT LEDGER |
