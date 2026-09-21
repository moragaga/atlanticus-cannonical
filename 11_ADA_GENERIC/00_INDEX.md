# ADA Generic — Canonical Index

Estado: **CURRENT / STAGE 1 CLOSED**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_SCOPE.md` | Qué es y qué no es ADA Generic; frontera de entrega al desarrollador. | CURRENT |
| `02_CURRENT_COMPOSITION.md` | Bootstrap y composición CURRENT. | VERIFIED / CURRENT |
| `03_CONFIGURATION_TO_RUNTIME.md` | Projection durable → runtime → Collector → browser stores. | CURRENT |
| `04_COLLECTOR_BOUNDARY.md` | Contrato implementado del Collector y frontera Web. | CLOSED / CURRENT |
| `05_FIRST_DELIVERABLE_VERTICAL.md` | Diferencia entre Stage 1 genérico y Golden Path de Tool real. | CURRENT |
| `06_SOURCE_LEDGER.md` | Fuentes/checkpoints/cutovers relevantes. | AUDIT LEDGER |
| `07_TOOL_DELIVERY_ORDER.md` | Orden funcional de Tools. | CURRENT |

Precondiciones y composición cerradas:

```text
Storage namespace ADA                         CLOSED / CURRENT
Tool Projection local/cosmos                  CLOSED / CURRENT
Tool persistence provider composition         CLOSED / CURRENT
KPI Registry durable                          CLOSED / CURRENT
KPI Definition durable                        CLOSED / CURRENT
ADA Web KPI Collector capability              CLOSED / CURRENT
ADA Generic operational bootstrap             CLOSED / CURRENT
ADA Generic Collector runtime wiring          CLOSED / CURRENT
Operational Render structural cutover         CLOSED / CURRENT
ADA Generic Stage 1                           CLOSED / CURRENT
```

## Frontera final de Stage 1

```text
Tool Projection durable
→ ToolStructure
→ KPI Collector
→ process cache
→ browser dcc.Store / ToolComponent
→ developer handoff
```

ADA Generic no construye obligatoriamente el body específico de una Tool.

La visualización concreta pertenece al consumidor/desarrollador.

## Siguiente foco único

```text
ADA-COMMAND-CENTER-ALARM-CONFIGURATION
PLANNED / NEXT
```

No reabrir ADA Generic sin finding concreto.
