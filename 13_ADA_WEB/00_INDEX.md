# ADA Web — Canonical Index

Estado: **CURRENT / STAGE 1 CLOSED / STARTER PORTABLE HISTORICALLY VERIFIED / RESOURCE PREPARATION 001 LOCAL VERIFIED / HOME E2E OPEN**  
Corte local y repositorio: `moragaga/atlanticus@da75752e87036b8318f38f8d405c55e8cb18717d`. La suite y ensayos Docker del incremento 001 son evidencia reportada para el artifact previo a publicar el commit, no CI repetido sobre un checkout limpio del SHA.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CURRENT_BASELINE.md` | Baseline operacional de un checkpoint anterior. | CURRENT / HISTORICAL |
| `02_TESTING_POLICY.md` | Pruebas funcionales; CSS/apariencia mediante validación visual. | CURRENT |
| `03_SHELL_BOUNDARIES.md` | Manager y Operational Shell separados; brecha Starter documentada en checkpoint anterior. | HISTORICAL + CURRENT BOUNDARY |
| `04_QUALIFICATION_HISTORY.md` | Historial previo de Web Starter/portabilidad/Docker. | HISTORICAL EVIDENCE |
| `05_SOURCE_LEDGER.md` | Evidencia anterior; más detalle en `17_DISTRIBUTION_AND_TOOLING/09_SOURCE_LEDGER.md`. | HISTORICAL |
| `06_INFRASTRUCTURE_STARTUP.md` | Bootstrap Web, collector y Resource Preparation; evidencia reciente de emuladores y límite de cold start. | CURRENT / LOCAL RESOURCE SCOPE CLOSED / COLD START OPEN |
| `07_ALARM_MANAGEMENT_FRONTEND.md` | UI alarm management sobre contrato del backend correspondiente. | OTHER FOCUS |

El incremento `RESOURCE-PREPARATION-001` está incorporado en `da75752`: la distribución construida, los recursos creados desde cero, los estados estructurados, la repetición idempotente y la recuperación de topología tras caída Cosmos/Azurite se observaron localmente. Eso no demuestra proyección funcional de Master ni Azure productiva.

**OPEN:** el Home visible con Cosmos vacío o caído, recuperación real de collectors a través del navegador, `/health/ready` con checks funcionales (se observó `checks: {}`), cold start bajo caída de Cosmos y prueba de rutas funcionales de Starter. Próximo trabajo **fuera de este índice operativo**: Master Projection externa, luego vínculo organizacional de usuarios ADA y después estabilidad Home como incrementos independientes.
