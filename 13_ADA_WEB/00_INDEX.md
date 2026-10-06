# ADA Web — Canonical Index

Estado: **CURRENT — INTEGRATED OPERATIONS FOUNDATION CURRENT / STATIC ALARM BASELINE INTEGRATION VERIFIED**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CURRENT_BASELINE.md` | Baseline Web funcional actual. | CURRENT |
| `02_TESTING_POLICY.md` | Política de pruebas. | CURRENT |
| `03_SHELL_BOUNDARIES.md` | Shell boundaries. | CURRENT/HISTORICAL |
| `04_QUALIFICATION_HISTORY.md` | Evidencia histórica. | HISTORICAL |
| `05_SOURCE_LEDGER.md` | Provenance del baseline Web. | AUDIT LEDGER |
| `06_INFRASTRUCTURE_STARTUP.md` | Runtime Docker, resource preparation y emuladores. | CURRENT |
| `07_ALARM_MANAGEMENT_FRONTEND.md` | Alarm management UI. | OTHER FOCUS |
| `08_INTEGRATED_OPERATIONS_PRESENTATION.md` | Specialized application foundation, Dashboard/Mine/Plant boundary y static baseline. | CURRENT / PARTIAL PRODUCT IMPLEMENTATION |

## CURRENT Web structure relevant to Integrated Operations

```text
Tool Projection READY
→ ToolStructure
→ OperationalRenderBinding
→ same binding to:
     Generic static Alarm Baseline
     Integrated Operations DashboardContext
→ one Dashboard page /
→ internal Mine + Plant surfaces
```

## Static baseline status

```text
contract implementation                    CURRENT / CLOSED
automated baseline qualification            VERIFIED
Integrated Operations browser observation   VERIFIED
Process visual variants                     OPEN
dynamic alarm overlay                       PLANNED / SEPARATE
```

## Integrated Operations foundation status

```text
specialized descriptor          CURRENT
additive Generic extension      CURRENT
Dashboard application module    CURRENT
Mine/Plant internal composition CURRENT
local emulator harness          CURRENT
product resource command        CURRENT
KPI-driven feature modules      PLANNED
```

## Reference

`isolated-web-functions/operational_trace` remains a visual/interaction reference only.

Do not reopen Alarm Engine domain rules while advancing Integrated Operations presentation.
