# ADA Generic — Canonical Index

Estado: **CURRENT DIRECTION**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_SCOPE.md` | Qué es y qué no es ADA Generic. | CURRENT |
| `02_CURRENT_COMPOSITION.md` | Capacidades que compone hoy `main`. | VERIFIED |
| `03_CONFIGURATION_TO_RUNTIME.md` | Tool → KPI Registry/Definition → backend outputs → collector. | CURRENT |
| `04_COLLECTOR_BOUNDARY.md` | Contrato implementado del Collector y siguiente wiring. | CLOSED / CURRENT |
| `05_FIRST_DELIVERABLE_VERTICAL.md` | Vertical inicial. | PROPOSED |
| `06_SOURCE_LEDGER.md` | Fuentes/checkpoints/cutovers relevantes. | AUDIT LEDGER |
| `07_TOOL_DELIVERY_ORDER.md` | Orden funcional de Tools. | CURRENT |

Precondiciones ya cerradas:

```text
KPI Registry durable                  CLOSED / CURRENT
KPI Definition durable                CLOSED / CURRENT
KPI Runtime recovery                  CLOSED / CURRENT
KPI Latest Delivery Registry consumer CLOSED / CURRENT
KPI Timeseries Registry consumer      CLOSED / CURRENT
KPI Historian recovery                CLOSED / CURRENT
ADA Web KPI Collector capability      CLOSED / CURRENT
```

Siguiente foco único:

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

El próximo chat debe integrar el collector existente. No reabrir su diseño.
