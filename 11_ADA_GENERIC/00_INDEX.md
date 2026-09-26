# ADA Generic — Canonical Index

Estado: **CURRENT / STAGE 1 + NAVIGATION LOCAL QUALIFICATION CLOSED**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_SCOPE.md` | Propósito y frontera genérica con consumidores concretos. | CURRENT |
| `02_CURRENT_COMPOSITION.md` | Bootstrap, Collector, Manager y Navigation. | CURRENT |
| `03_CONFIGURATION_TO_RUNTIME.md` | Projection durable → Collector → browser stores. | CURRENT |
| `04_COLLECTOR_BOUNDARY.md` | Contrato congelado del Collector. | CLOSED / CURRENT |
| `05_FIRST_DELIVERABLE_VERTICAL.md` | Stage 1, qualification local y Golden Path pendiente. | CURRENT |
| `06_SOURCE_LEDGER.md` | Commits y evidencia exacta de este frente. | AUDIT LEDGER |
| `07_TOOL_DELIVERY_ORDER.md` | Orden funcional de Tools. | CURRENT |

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP               CLOSED / CURRENT
ADA-GENERIC-COLLECTOR-RUNTIME-WIRING            CLOSED / CURRENT
ADA-GENERIC-STAGE-1                             CLOSED / CURRENT
ADA-GENERIC-NAVIGATION-COMPOSITION              CLOSED / CURRENT
ADA-GENERIC-NAVIGATION-LOCAL-E2E                CLOSED / VERIFIED MANUAL
ADA-GENERIC-NAVIGATION-CLIENT-REGRESSION        CLOSED / VERIFIED MANUAL
REAL DURABLE LOCAL DOCKER/AZURE QUALIFICATION   PLANNED / UNVERIFIED
WEB DISTRIBUTABLE ARTIFACT QUALIFICATION        PLANNED / UNVERIFIED
FIRST REAL TOOL GOLDEN PATH                    PLANNED / OPEN
```

El Collector ya existe. No proyectar un nuevo Collector como requisito de distribución.
La frontera genérica de datos termina en `dcc.Store` por ToolComponent; una Tool concreta
requiere configuración y puede requerir visualización propia. La próxima revisión de
distribución reutiliza el código y los generadores existentes antes de decidir cambios.
