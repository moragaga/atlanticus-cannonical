# Atlanticus Canonical Context — Index

Estado: **CURRENT — KPI BACKEND CLOSED; COMMAND CENTER / ALARM ANALYSIS NEXT (2026-10-03)**

## Autoridad de este cierre

```text
Implementation        moragaga/atlanticus@2505196019fcc51e5f97ff66a3159beb87fe71f0
Canonical pre-replace moragaga/atlanticus-cannonical@38404e61c69978183cd515be4ca40afed7ef59e8
Decisions             NOT INSPECTED in this closure by explicit instruction
```

Git permanece **SOLO LECTURA**.

## Estado por frente

| Ubicación | Estado relevante |
|---|---|
| `01_CURRENT_STATE.md` | Users/Web state remains CURRENT; KPI backend History/Historian/Timeseries closure incorporated. |
| `08_ROADMAP.md` | Command Center / Alarm backend analysis is the next unique focus. |
| `09_OPEN_QUESTIONS.md` | KPI operational E2E remains BLOCKED by required Web corrections; Navigation/artifacts remain separate planned work. |
| `04_ALARM_ENGINE/` | Existing canonical input for the next analysis; not modified by this closure. |
| `14_ADA_COMMAND_CENTER/` | Existing canonical input for the next analysis; not modified by this closure. |
| `16_KPI_BACKEND_RECOVERY/` | KPI Materialization + Latest + Historian + Timeseries multi-Tool CLOSED / VERIFIED locally. |
| `17_DISTRIBUTION_AND_TOOLING/` | Separate planned front; not reopened here. |

## Checkpoints

```text
KPI-NAMED-CONNECTIONS                    CLOSED / VERIFIED / CURRENT
KPI-REGISTRY-MATERIALIZATION             CLOSED / VERIFIED / CURRENT
KPI-LATEST-MULTI-TOOL-DELIVERY           CLOSED / VERIFIED / CURRENT
KPI-HISTORIAN-ROLLING-READ-MODEL         CLOSED / VERIFIED / CURRENT
KPI-TIMESERIES-MULTI-TOOL-DELIVERY       CLOSED / VERIFIED / CURRENT
KPI-HISTORY-DATASET-BOUNDARY              CLOSED / VERIFIED / CURRENT

KPI-FULL-OPERATIONAL-E2E                  BLOCKED
COMMAND-CENTER-ALARM-BACKEND-ANALYSIS     PLANNED / NEXT
```

## Siguiente frontera única

Analizar `ada-command-center` y Alarmas en backend usando:

```text
moragaga/atlanticus:main
atlanticus-cannonical/04_ALARM_ENGINE/
atlanticus-cannonical/14_ADA_COMMAND_CENTER/
```

El objetivo del siguiente frente es determinar el estado real, las fronteras y si Alarmas debe madurar a un engine reusable.

No mezclar en ese incremento:

```text
KPI backend
KPI E2E
artifact generation
.env.detail
distribution
Navigation access
```
