# ADA Web — Canonical Index

Estado: **CURRENT — STATIC ALARM BASELINE CLOSED / VISUAL QUALIFICATION OPEN**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CURRENT_BASELINE.md` | Baseline Web funcional actual. | CURRENT |
| `02_TESTING_POLICY.md` | Política de pruebas. | CURRENT |
| `03_SHELL_BOUNDARIES.md` | Shell boundaries. | CURRENT/HISTORICAL |
| `04_QUALIFICATION_HISTORY.md` | Evidencia histórica. | HISTORICAL |
| `05_SOURCE_LEDGER.md` | Provenance del baseline Web. | AUDIT LEDGER |
| `06_INFRASTRUCTURE_STARTUP.md` | Runtime Docker, resource preparation y emuladores. | CURRENT / VERIFIED |
| `07_ALARM_MANAGEMENT_FRONTEND.md` | Alarm management UI. | OTHER FOCUS |
| `08_INTEGRATED_OPERATIONS_PRESENTATION.md` | Integrated Operations composition/focus + CURRENT static baseline boundary. | CURRENT DESIGN / PARTIAL IMPLEMENTATION |

## CURRENT Web structure relevant to alarms

```text
Tool Projection READY
→ ToolStructure + ToolRenderTopology
→ OperationalRenderBinding
→ AlarmBaselineProjection
→ AlarmBaselineSurface
→ ADA Generic layout
```

## Static baseline status

```text
contract implementation    CURRENT / CLOSED
automated qualification    VERIFIED
visual browser qualification OPEN
dynamic alarm overlay      PLANNED / SEPARATE
```

## Reference

`isolated-web-functions/operational_trace` remains a visual/interaction reference only.

## Current sequence for this track

```text
1. visual qualification of static baseline
2. only after that, dynamic alarm overlay against authoritative live projection
```

Do not reopen Alarm Engine domain rules during the visual qualification increment.
