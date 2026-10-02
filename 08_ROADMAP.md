# Atlanticus — Roadmap

Estado: **CURRENT ROADMAP BY FRONT — DISTRIBUTION TOOLING CLOSED**

## CLOSED — Web distribution/tooling

```text
WEB-DISTRIBUTION-SHARED-ENGINE-CLEANUP          CLOSED
ADA-STARTER-RUNTIME-THINNING                    CLOSED
ADA-MASTER-PROJECTION-RUNTIME-OWNERSHIP         CLOSED
COMMAND-CENTER-DISTRIBUTION-PROFILE             CLOSED
CROSS-PLATFORM-WHEELHOUSE-SDIST-FALLBACK        CLOSED
```

Current distribution outcomes:

```text
generic         PASS
ada             PRECHECK_PASS
command-center  PRECHECK_PASS
```

## NEXT único

```text
ADA-AND-COMMAND-CENTER-ENV-DETAIL-CONTRACT
PLANNED / NEXT
```

Objetivo:

```text
ADA .env.detail
Command Center .env.detail
    ↓
manual vs derived values
secret vs non-secret
local vs production
Storage final/durable
Cosmos local
Master Projection
```

No mezclar todavía KPI Collector ni rediseño UI.

## AFTER NEXT

Una vez congelado el contrato de configuración:

```text
DUAL-APP-STORAGE-FINAL-COSMOS-LOCAL-SMOKE
PLANNED
```

Levantar:

```text
ADA Generic
ADA Command Center
```

con Storage final y Cosmos local para validar transición/reconstrucción mediante Master Projection.

Command Center requiere antes completar su integración de Master Projection y cualquier runtime/configuration gap revelado por el contrato.

## THEN — ADA only

Después del smoke dual:

```text
ADA-KPI-COLLECTOR-OPERATIONAL-E2E
PLANNED

ADA-UI-RECONSTRUCTION
PLANNED
```

Objetivo KPI:

```text
real Tool/KPI data
→ KPI Delivery Latest/Timeseries
→ Collector
→ browser stores
→ UI
```

No reabrir Command Center dentro de ese incremento.

## UI design constraint

ADA dispone de modo `AUTHORING` para suprimir overlays degradados mientras se diseña.

OPEN posterior:

```text
absence of first observation
!= necessarily source error
```

El modelo actual no tiene `NO_DATA`; resolver semántica después de observar el flujo Collector real.
