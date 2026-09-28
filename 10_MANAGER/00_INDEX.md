# Manager — Canonical Index

Estado: **CURRENT / USERS RECOVERY + INTERNAL USERS PROJECTION CLOSED FOR VALIDATED SCOPE / MASTER SEPARATE NEXT**  
Implementación revisada: `moragaga/atlanticus@208c8d6244795ba92cbe6f8e6b11e9743191367d`; no atribuir qualification global al commit.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager genérico sin dependencia rígida de ADA. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar, header y navegación. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | Diferencia `ManagerModule`/`ManagerEntry`, Source/Projection y sesión. | CURRENT; checkpoints históricos |
| `04_TOOL_CONFIGURATION.md` | Tool Configuration, Source/Projection. | CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Consumo Source/Projection desde Manager. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Pruebas de contratos y límite de tests visuales. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Evidencia histórica de UI e integración local. | HISTORICAL |
| `08_BOOTSTRAP_AND_ACCESS.md` | Acceso vigente, Users Recovery y separación de Master externo. | CURRENT + NEXT |
| `09_ADA_COMPONENT_LINKS.md` | Links/warmup histórico del componente. | CURRENT BASELINE; revisar en propio contrato |
| `10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md` | Datos operacionales ADA por usuario. | PLANNED / OTHER FOCUS |

## Estado

Manager y Users conservan ownership genérico. `ManagerAuthorizationPolicy` gobierna visibilidad y el servidor verifica las acciones. `/manager` sigue siendo Home; la navegación usa registry común. En Administración están Usuarios y **Proyección de usuarios** (`ManagerEntry` con `users.manage`). Esta última utiliza dos pestañas para capturar respaldos y comparar/aplicar restauración estricta o sustitución completa. No simular Source/Projection ordinario para Users.

**VERIFIED USER-REPORTED:** validación backend REPLACE en laboratorio Cosmos/Azurite, auditoría/before-image y posterior recaptura; tests seleccionados y aceptación de UI. **OPEN:** operación invasiva desde navegador, mantenimiento/revocación efectivos, interrupción real y full CI/Ruff final.

La página externa Master Projection **NO pertenece al Manager**: acceso por URL/credenciales propias para inicializar proyecciones cuando la autorización normal aún no existe. Diseño e implementación pendientes en `../15_WEB_PLATFORM/06_PRE_MANAGER_BOOTSTRAP_SURFACE.md`. No reabrir Manager core durante el siguiente foco.
