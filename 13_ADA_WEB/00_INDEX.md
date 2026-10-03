# ADA Web — Canonical Index

Estado: **CURRENT — DISTRIBUTED WEB RUNTIME VERIFIED / REAL CONFIGURATION PAUSED FOR ROOT CUTOVER**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CURRENT_BASELINE.md` | Baseline funcional actual. | CURRENT |
| `02_TESTING_POLICY.md` | Política de pruebas. | CURRENT |
| `03_SHELL_BOUNDARIES.md` | Shell boundaries. | CURRENT/HISTORICAL |
| `04_QUALIFICATION_HISTORY.md` | Evidencia histórica. | HISTORICAL |
| `05_SOURCE_LEDGER.md` | Evidencia previa. | HISTORICAL |
| `06_INFRASTRUCTURE_STARTUP.md` | Runtime Docker, resource preparation y emuladores. | CURRENT / VERIFIED |
| `07_ALARM_MANAGEMENT_FRONTEND.md` | Alarm management UI. | OTHER FOCUS |

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

## Current block before continuing UI configuration

```text
ADA-TOOL-SCOPED-CONFIGURATION-AND-USER-RUNTIME
```

The application should not continue accumulating real configuration until Tool-scoped ownership and the Users model are corrected.

Alarm integration remains later and separate.
