# Manager — Canonical Index

Estado: **CURRENT — checkpoint Master histórico preservado; ADA DATOS OPERACIONALES ACTUALIZADO 2026-09-29**  
Corte de reconciliación Master: `atlanticus@9c6daffd04b9c249f75a55b6cdb9b44e6d92a795`; canonical integrado en `atlanticus-cannonical@571f9c09e7968c9bc3563cdd6d2ff50c6ab21649`. Users conserva su qualification histórica propia; no se reejecutó una suite global en este cierre documental.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager genérico sin dependencia rígida de ADA. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar, header y navegación. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | Diferencia `ManagerModule`/`ManagerEntry`, Source/Projection y sesión. | CURRENT; checkpoints históricos |
| `04_TOOL_CONFIGURATION.md` | Tool Configuration, Source/Projection. | CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Consumo Source/Projection desde Manager. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Pruebas de contratos y límite de tests visuales. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Evidencia histórica de UI e integración local. | HISTORICAL |
| `08_BOOTSTRAP_AND_ACCESS.md` | Acceso, Users Recovery y Master externo con Apply individual de seis dominios. | CURRENT |
| `09_ADA_COMPONENT_LINKS.md` | Links/warmup histórico del componente. | CURRENT BASELINE; revisar en propio contrato |
| `10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md` | Implementación verificada de catálogo, asignaciones y Manager; distingue nueva UX futura. | CURRENT / VER DETALLE FOCAL ABAJO |
| `11_ADA_OPERATIONAL_DATA_ROADMAP.md` | Snapshot durable OPEN y secuencia incremental. | CURRENT PLAN |

## Estado del checkpoint Master anterior (histórico para Datos operacionales)

Manager y Users conservan ownership genérico. `ManagerAuthorizationPolicy` gobierna visibilidad y el servidor verifica las acciones. `/manager` sigue siendo Home; la navegación usa registry común. En Administración están Usuarios y **Proyección de usuarios** (`ManagerEntry` con `users.manage`). Esta última utiliza dos pestañas para capturar respaldos y comparar/aplicar restauración estricta o sustitución completa. No simular Source/Projection ordinario para Users.

**VERIFIED USER-REPORTED:** validación backend REPLACE en laboratorio Cosmos/Azurite, auditoría/before-image y posterior recaptura; tests seleccionados y aceptación de UI. **OPEN:** operación invasiva desde navegador, mantenimiento/revocación efectivos, interrupción real y full CI/Ruff final.

La página externa Master Projection **NO pertenece al Manager**: está implementada en la Web existente con acceso propio, inspección y Apply individual de seis proyecciones ordinarias bajo autorización, confirmación y revalidación de targets. Users REPLACE desde Master sigue BLOCKED. La qualification Docker positiva corresponde solo a Navigation en `ca3ee508`; la nueva distribución `9c6daffd` requiere prueba Docker propia.

La identificación operacional de usuarios ADA es otro dominio PLANNED, sin implementación en este corte: Asignaciones y Cargos, sin extender Atlanticus Users ni crear automáticamente una séptima proyección Master. El término *operational scope* permanece OPEN hasta determinar si corresponde a Área o representa otro concepto. La definición contractual y el backend deben preceder a la integración de su interfaz.

---

## Reconciliación específica ADA Datos operacionales — 2026-09-29

**SUPERSEDED para este dominio exclusivamente:** el párrafo y fila previos que calificaban la identificación operacional como «PLANNED / sin implementación» corresponden al checkpoint Master histórico descrito arriba; **no describen** `atlanticus:main@caced5d7711cf059d36ec61aecc9b3e9629bd41f`. La actualización focal no revalida Master/Users ni los otros archivos de este índice.

| Archivo | Contenido | Estado actualizado |
|---|---|---|
| `10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md` | Catálogo/assignments CURRENT, Source por catálogo y usuario, proyección Cosmos y Manager existente; UX nueva separada de código actual. | CURRENT / VERIFIED STATIC + LOCAL TESTS USER-REPORTED |
| `11_ADA_OPERATIONAL_DATA_ROADMAP.md` | Decisiones de flujo, contrato OPEN del snapshot y secuencia incremental. | CURRENT PLANNING / NEXT DESIGN |
| `../15_WEB_PLATFORM/14_ADA_OPERATIONAL_SESSION_AND_WARMUP.md` | Sesión individual + warmup solo catálogos, nunca usuarios ni asignaciones. | DECIDED / PLANNED IMPLEMENTATION |

**Evidencia:** 25 pruebas completas del scope operacional comunicadas PASS; 13 del Manager en gate separado; Ruff `I001` OPEN. No hay E2E Entra/Azure ni snapshot consolidado implementado. **Foco siguiente único del frente:** cerrar semántica del snapshot durable, sin mezclar modificación del Manager ni warmup.
