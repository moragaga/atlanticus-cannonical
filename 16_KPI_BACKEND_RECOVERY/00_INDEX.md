# KPI Backend Recovery / Materialization / Delivery — Index

Estado: **CURRENT — MATERIALIZATION + LATEST CLOSED; HISTORIAN ROLLING + TIMESERIES NEXT**

## Autoridad de este cierre

```text
Implementation
moragaga/atlanticus@a521e807d22451a9a4f86f11f07bcde7632b1a33

Decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical inspected before replacement
moragaga/atlanticus-cannonical@f0de87407c59181527a22737220d14f8e0d1e309
```

Git permanece **SOLO LECTURA**.

## Estado por capability

| Archivo | Estado |
|---|---|
| `02_REPROCESS_CONTRACT.md` | CURRENT; no reabierto en este hito. |
| `03_KPI_RUNTIME.md` | CLOSED / VERIFIED / CURRENT; no reabierto. |
| `04_LATEST_DELIVERY.md` | CLOSED / VERIFIED / CURRENT; reemplaza el diseño antiguo de Registry directo. |
| `05_HISTORIAN.md` | Durable history CURRENT; rolling read model 24 h / 30 s PLANNED. |
| `06_TIMESERIES_DELIVERY.md` | Implementación antigua CURRENT pero PLANNED REPLACEMENT. |
| `07_SAFETY_RULES.md` | CURRENT. |
| `08_CONFIGURATION.md` | CURRENT para named connections + readiness; Timeseries aún pendiente de migración. |
| `09_TESTING.md` | VERIFIED para Materialization + Latest; política de tests refinada. |
| `10_SOURCE_LEDGER.md` | CURRENT audit ledger de este hito. |
| `11_MATERIALIZATION.md` | NEW / CLOSED / VERIFIED / CURRENT. |

## Checkpoints

```text
KPI-NAMED-CONNECTIONS                    CLOSED / VERIFIED / CURRENT
KPI-REGISTRY-MATERIALIZATION             CLOSED / VERIFIED / CURRENT
KPI-LATEST-MULTI-TOOL-DELIVERY           CLOSED / VERIFIED / CURRENT
KPI-READINESS-HARDENING                  CLOSED / VERIFIED / CURRENT

KPI-HISTORIAN-ROLLING-READ-MODEL         PLANNED / NEXT
KPI-TIMESERIES-MULTI-TOOL-DELIVERY       PLANNED / AFTER HISTORIAN ROLLING
```

## Siguiente frontera única

Cerrar contrato e implementar **Historian rolling read model** antes de modificar Timeseries Delivery.

No reabrir Latest Delivery salvo finding real de contradicción con el contrato compartido.
