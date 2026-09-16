# ADA Generic — Canonical Index

Estado: **CURRENT DIRECTION**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_SCOPE.md` | Qué es y qué no es ADA Generic; ownership ADA vs infraestructura genérica. | CURRENT |
| `02_CURRENT_COMPOSITION.md` | Capacidades que compone hoy `main`. | VERIFIED |
| `03_CONFIGURATION_TO_RUNTIME.md` | Cadena Tool → KPI → runtime y dependencias Projection. | FROZEN SEMANTICS + CONFIGURATION CONTRACTS CURRENT |
| `04_COLLECTOR_BOUNDARY.md` | Semántica de Collector y mapeo físico pendiente. | FROZEN SEMANTICS |
| `05_FIRST_DELIVERABLE_VERTICAL.md` | Candidato de vertical para primer entregable. | PROPOSED |
| `06_SOURCE_LEDGER.md` | Fuentes, checkpoints y cutovers relevantes. | AUDIT LEDGER |
| `07_TOOL_DELIVERY_ORDER.md` | Operaciones Integradas primero; Mina después. | CURRENT |

La cadena Configuration relevante está sobre contratos Source/Projection genéricos sin cambiar ownership ADA:

```text
Tools Source/Projection              CLOSED / CURRENT
KPI Configuration Source/Projection CLOSED / CURRENT
KPI Definition Source/Projection    CLOSED / CURRENT
```

Siguiente frontera administrativa:

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

ADA Generic runtime no debe usarse para reintroducir un `KpiDefinitionAuthority` ni revision identities privadas ya removidas.
