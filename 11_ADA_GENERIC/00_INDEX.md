# ADA Generic — Canonical Index

Estado: **CURRENT — 0.2.26 DISTRIBUTED RUNTIME VERIFIED / TOOL-SCOPED CUTOVER NEXT**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_SCOPE.md` | Frontera de ADA Generic. | CURRENT |
| `02_CURRENT_COMPOSITION.md` | Product root, runtime y shared Master Projection. | CURRENT |
| `03_CONFIGURATION_TO_RUNTIME.md` | Environment/persistence y namespace. | CURRENT / CUTOVER PLANNED |
| `04_COLLECTOR_BOUNDARY.md` | Contrato Collector. | CLOSED / CURRENT |
| `05_FIRST_DELIVERABLE_VERTICAL.md` | Golden Path. | CURRENT / OPEN |
| `06_SOURCE_LEDGER.md` | Historial. | HISTORICAL |
| `07_TOOL_DELIVERY_ORDER.md` | Orden funcional y siguiente frontera. | CURRENT |

## Estado

```text
ADA Generic                         0.2.26 / CURRENT
distributed consumer runtime        VERIFIED / CLOSED
Cosmos Data Explorer                VERIFIED / CLOSED
Tool-scoped configuration cutover   PLANNED / NEXT
recovery on corrected model         BLOCKED UNTIL CUTOVER
UI / Alarm integration              PLANNED / LATER
```

## Regla

ADA conserva composición de producto.

No reintroducir motores genéricos dentro de ADA y no mantener contratos legacy durante el cutover Tool-scoped.
