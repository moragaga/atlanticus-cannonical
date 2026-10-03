# ADA Web — Canonical Index

Estado: **CURRENT — TOOL CONTRACT WEB CUTOVER CLOSED / DISTRIBUTION PREPARATION NEXT**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CURRENT_BASELINE.md` | Baseline funcional actual. | CURRENT |
| `02_TESTING_POLICY.md` | Política de pruebas. | CURRENT |
| `03_SHELL_BOUNDARIES.md` | Shell boundaries. | CURRENT/HISTORICAL |
| `04_QUALIFICATION_HISTORY.md` | Evidencia histórica. | HISTORICAL |
| `05_SOURCE_LEDGER.md` | Evidencia previa. | HISTORICAL |
| `06_INFRASTRUCTURE_STARTUP.md` | Runtime Docker, resource preparation y emuladores. | CURRENT / VERIFIED |
| `07_ALARM_MANAGEMENT_FRONTEND.md` | Alarm management UI. | OTHER FOCUS |
| `08_INTEGRATED_OPERATIONS_PRESENTATION.md` | Integrated Operations composition, focus, responsive y videowall presentation contract. | CURRENT DESIGN / IMPLEMENTATION PLANNED |

## Runtime verified

```text
consumer repo isolated
Docker image build PASS
Azurite PASS
Cosmos Emulator PASS
Data Explorer PASS
resource preparation PASS
Web healthy
/health/live 200
/health/ready 200
```

## Current sequence

Tool Contract Web Cutover is closed.

Distribution preparation remains the implementation priority documented in `01_CURRENT_BASELINE.md`.

Integrated Operations now has a canonical presentation design contract, but its implementation and visual qualification remain PLANNED.

Alarm Engine / Command Center integration remains later and separate.
