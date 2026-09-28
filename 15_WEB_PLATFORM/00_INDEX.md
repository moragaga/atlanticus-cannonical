# Atlanticus Web Platform — Canonical Index

Estado: **CURRENT / USERS RECOVERY + MANAGER UI VALIDATED SCOPE CLOSED / MASTER PROJECTION NEXT**  
Corte estático: `moragaga/atlanticus@208c8d6244795ba92cbe6f8e6b11e9743191367d`; decisiones `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`; versión reemplazada de canonical `e49901fb3ceef5431edbde1d0dcbb29fc3502855`. Qualification operacional según evidencias reportadas para este hito, no certificación global del commit.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CAPABILITY_INDEPENDENCE.md` | Ownership genérico de capabilities Web. | CURRENT |
| `02_USER_ACTIVITY_HISTORY.md` | Dirección histórica de actividad/sesión. | CURRENT DIRECTION |
| `03_RESOURCE_PROVISIONING.md` | Plan parcial de recursos Manager; plan global separado. | CURRENT + SEPARATE |
| `04_WEB_READINESS_AND_DECOUPLING.md` | Inicio resiliente y desacoplamiento. | CURRENT |
| `05_DEPLOYMENT_ORDER.md` | Secuencia Cloud/Web/resources/Sources/projections/Backend; no convertirla en orden artificial de targets. | CURRENT DIRECTION |
| `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md` | Próxima **página externa Master Projection**; credenciales mediante tooling y dos estados según presencia del archivo. | PLANNED / NEXT |
| `07_PROJECTION_ORCHESTRATION.md` | Targets exactos, excepción CURRENT de Users y reutilización futura por Master. | CURRENT + PLANNED |
| `08_EXTERNAL_RESOURCE_REQUIREMENTS.md` | Conexiones nombradas y ownership de recursos ajenos. | CONTRACT DESIGN |
| `09_CURRENT_GAPS.md` | Checkpoint histórico de Starter anterior. | HISTORICAL; NO CURRENT LEDGER |
| `10_SOURCE_LEDGER.md` | Referencias/evidencia histórica. | HISTORICAL |
| `11_OPEN_ITEMS.md` | Pendientes, seguridad y único siguiente foco. | CURRENT |
| `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` | Ownership y contratos históricos vigentes de Users/Profiles/Access/Navigation; el delta Users Projection está en 13. | CURRENT BASELINE + DELTA 13 |
| `13_USERS_PROJECTION_RECOVERY.md` | Snapshot aprobado, RESTORE/REPLACE backend y pantalla interna de proyección Users, evidencia y límites. | CURRENT / CLOSED VALIDATED SCOPE |

## Estado de frontera

**CURRENT:** Blob Registry + Cosmos promovidos; Users Recovery con `before-image`/auditoría; UI interna de Users Projection como `ManagerEntry` con pestañas y modal. El `application_namespace` gobierna el registro lógico; `tool_namespace` no crea otro conjunto de Users. No confundir Users Projection de Manager con Master Projection externa.

**VERIFIED USER-REPORTED:** REPLACE desde CLI en laboratorio Docker, auditoría/respaldo previo y nueva captura válida; tests seleccionados tras correctivo 007 y aceptación de la pantalla. **UNVERIFIED:** recuperación tras fallo parcial real, revocación de sesiones, ejecución invasiva desde navegador, full CI/Ruff final/Azure productivo.

**NEXT exclusivo:** contrato e implementación incremental de Master Projection fuera del Manager usando los servicios actuales y tooling ADA existente. Su página debe manejar ausencia de archivo sin errores indebidos ni habilitar acceso; cuando el archivo exista e integro y se autentique el operador, podrá operar proyecciones aplicables. Diseño criptográfico e integración de warmup **OPEN**. No inventar schemas/rutas/nombres de archivo durante el cierre documental.
