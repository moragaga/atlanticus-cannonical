# KPI Backend Recovery / Registry Consumption — Index

Estado: **CLOSED / VERIFIED / CURRENT**

Autoridad implementada para este dominio:

```text
moragaga/atlanticus@3ca8c833df916a4e0812c76eaba84ee5fde8a1cc
```

Los contracts backend cerrados aquí permanecen vigentes en el checkpoint global
`d484569cbe0290f38f239481cde81b13a23deecf`.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_PROBLEM.md` | Problema que motivó recovery y cutover. | CLOSED / HISTORICAL CONTEXT |
| `02_REPROCESS_CONTRACT.md` | Semántica implementada de reprocess autorizado. | CURRENT |
| `03_KPI_RUNTIME.md` | Runtime forced-current. | CLOSED / VERIFIED / CURRENT |
| `04_LATEST_DELIVERY.md` | Registry consumer + owned output. | CLOSED / VERIFIED / CURRENT |
| `05_HISTORIAN.md` | Historian full replay CURRENT. | CLOSED / VERIFIED / CURRENT |
| `06_TIMESERIES_DELIVERY.md` | Registry consumer + owned output. | CLOSED / VERIFIED / CURRENT |
| `07_SAFETY_RULES.md` | Gates preservados. | CURRENT |
| `08_CONFIGURATION.md` | Flags/ENV/container ownership. | CURRENT |
| `09_TESTING.md` | Qualification observada. | VERIFIED |
| `10_SOURCE_LEDGER.md` | Código CURRENT inspeccionado. | AUDIT LEDGER |

La frontera siguiente ya no es cerrar Collector como capability; eso quedó completado en
`d484569...`.

Siguiente frontera:

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

El backend KPI no debe modificarse para resolver ese wiring salvo conflicto nuevo demostrado.
