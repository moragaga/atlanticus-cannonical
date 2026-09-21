# ADA Generic — Canonical Index

Estado: **CURRENT DIRECTION**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_SCOPE.md` | Qué es y qué no es ADA Generic. | CURRENT |
| `02_CURRENT_COMPOSITION.md` | Composición CURRENT y gap de bootstrap. | VERIFIED |
| `03_CONFIGURATION_TO_RUNTIME.md` | Projection durable → runtime → Collector. | CURRENT |
| `04_COLLECTOR_BOUNDARY.md` | Contrato implementado del Collector. | CLOSED / CURRENT |
| `05_FIRST_DELIVERABLE_VERTICAL.md` | Vertical inicial. | PROPOSED |
| `06_SOURCE_LEDGER.md` | Fuentes/checkpoints/cutovers relevantes. | AUDIT LEDGER |
| `07_TOOL_DELIVERY_ORDER.md` | Orden funcional de Tools. | CURRENT |

Precondiciones cerradas:

```text
Storage namespace ADA                      CLOSED / CURRENT
Tool Projection local/cosmos               CLOSED / CURRENT
Tool persistence provider composition      CLOSED / CURRENT
KPI Registry durable                       CLOSED / CURRENT
KPI Definition durable                     CLOSED / CURRENT
ADA Web KPI Collector capability           CLOSED / CURRENT
```

Siguiente foco único:

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

No reabrir Source/Projection/namespace/Collector.

El siguiente chat debe reemplazar la ruta startup in-process de Tool por el consumo de la
composición durable ya implementada y mantener la aplicación ejecutable en estados
`READY | UNCONFIGURED | UNAVAILABLE | INVALID`.
