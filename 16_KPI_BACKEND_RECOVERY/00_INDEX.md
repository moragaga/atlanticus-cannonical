# KPI Backend Recovery / Materialization / Delivery — Index

Estado: **CURRENT — MATERIALIZATION + LATEST + HISTORIAN ROLLING CLOSED; TIMESERIES NEXT**

## Autoridad de este cierre

```text
Implementation
moragaga/atlanticus@38bcd8c5607d67f999e2bc4bf9dbf176c8340588

Decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical inspected before replacement
moragaga/atlanticus-cannonical@bac346a4c65e7a7d689e74a421656e75fe1d27b3
```

Git permanece **SOLO LECTURA**.

## Estado por capability

| Archivo | Estado |
|---|---|
| `02_REPROCESS_CONTRACT.md` | CURRENT; no reabierto en este hito. |
| `03_KPI_RUNTIME.md` | CLOSED / VERIFIED / CURRENT; no reabierto. |
| `04_LATEST_DELIVERY.md` | CLOSED / VERIFIED / CURRENT; no reabierto. |
| `05_HISTORIAN.md` | CLOSED / VERIFIED / CURRENT; durable history + rolling Timeseries read model. |
| `06_TIMESERIES_DELIVERY.md` | Implementación antigua CURRENT; reemplazo multi-Tool sobre rolling PLANNED / NEXT. |
| `07_SAFETY_RULES.md` | CURRENT. |
| `08_CONFIGURATION.md` | CURRENT para named connections + readiness; Timeseries aún pendiente de migración. |
| `09_TESTING.md` | VERIFIED para Materialization + Latest + Historian rolling; Timeseries nuevo aún UNVERIFIED. |
| `10_SOURCE_LEDGER.md` | CURRENT audit ledger de este cierre. |
| `11_MATERIALIZATION.md` | CLOSED / VERIFIED / CURRENT. |

## Checkpoints

```text
KPI-NAMED-CONNECTIONS                    CLOSED / VERIFIED / CURRENT
KPI-REGISTRY-MATERIALIZATION             CLOSED / VERIFIED / CURRENT
KPI-LATEST-MULTI-TOOL-DELIVERY           CLOSED / VERIFIED / CURRENT
KPI-READINESS-HARDENING                  CLOSED / VERIFIED / CURRENT
KPI-HISTORIAN-ROLLING-READ-MODEL         CLOSED / VERIFIED / CURRENT

KPI-TIMESERIES-MULTI-TOOL-DELIVERY       PLANNED / NEXT
```

## Contrato upstream ya disponible para Timeseries

Historian publica una proyección local regenerable:

```text
<application_root>/timeseries/current.parquet
```

Contrato CURRENT:

```text
maximum physical horizon = 24 h
grid                      = 30 s
shape                     = wide
timestamp                 = UTC
write                     = atomic replacement
authority                 = durable history + HistorianAuthority
```

El rolling contiene solo cobertura física observada. La hidratación de la grilla lógica y los `null`
faltantes pertenecen a Timeseries Delivery.

## Siguiente frontera única

Reemplazar **KPI Timeseries Delivery** para consumir:

```text
materialized Registry
+
named connections
+
HistorianAuthority
+
Historian rolling current.parquet
```

No reabrir Historian, Latest o Materialization salvo finding real de incompatibilidad contractual.
