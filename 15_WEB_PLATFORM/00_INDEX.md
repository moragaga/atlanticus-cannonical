# Atlanticus Web Platform — Canonical Index

Estado: **CURRENT — USERS RECOVERY EN SU ALCANCE HISTÓRICO; MASTER 001A–001D.2 IMPLEMENTADO; 001D.3 DOCKER NAVIGATION CLOSED; 001D.4 SYNC LOCAL CLOSED**  
Corte funcional Master: `atlanticus@9c6daffd04b9c249f75a55b6cdb9b44e6d92a795`; HEAD posterior `80748a21735a91d04f520ed4a8b9abf9dc9421da` solo añade cambios de `kpi-runtime` en el delta consultado. Canonical con los siete reemplazos integrado en `571f9c09e7968c9bc3563cdd6d2ff50c6ab21649`; decisions en `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. La igualdad byte a byte frente a candidatos originales y los DOCX históricos completos de identidad no están verificados.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CAPABILITY_INDEPENDENCE.md` | Ownership genérico de capabilities Web. | CURRENT / NO MODIFICADO |
| `02_USER_ACTIVITY_HISTORY.md` | Dirección histórica de actividad/sesión. | CURRENT DIRECTION / NO MODIFICADO |
| `03_RESOURCE_PROVISIONING.md` | Resource Preparation 001 y qualification local parcial. | CURRENT / LOCAL SCOPE CLOSED / NO REABIERTO |
| `04_WEB_READINESS_AND_DECOUPLING.md` | Estado Tool, collector y Home degradado. | CURRENT + OPEN DEFERRED |
| `05_DEPLOYMENT_ORDER.md` | Web/recursos/proyección/backend; Cloud E2E pendiente. | CURRENT DIRECTION |
| `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md` | Master protegido con preview y Apply individual mediante confirmación. | CURRENT IMPLEMENTED / OPERATIONS OPEN |
| `07_PROJECTION_ORCHESTRATION.md` | Seis targets exactos y dependencias; executor 001D.1 y HTTP 001D.2; Users separado y no ejecutable. | CURRENT / LOCAL QUALIFICATION PARTIAL |
| `08_EXTERNAL_RESOURCE_REQUIREMENTS.md` | Conexiones nombradas y ownership de recursos externos. | CONTRACT DESIGN / NO MODIFICADO |
| `09_CURRENT_GAPS.md` | Checkpoint histórico de Starter de un SHA anterior. | HISTORICAL / NO USAR COMO ESTADO MASTER |
| `10_SOURCE_LEDGER.md` | Historial y referencias de otros hitos Web. | HISTORICAL / NO REVALIDADO GLOBALMENTE |
| `11_OPEN_ITEMS.md` | Cierre acotado 001D, Docker de la distribución nueva y otros gates Master/Users. | CURRENT / OPEN GATES |
| `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` | Ownership genérico de Users/Profiles/Access/Navigation. | CURRENT BASELINE |
| `13_USERS_PROJECTION_RECOVERY.md` | Snapshots aprobados y recuperación especial dentro del Manager. | CURRENT / SCOPE CLOSED + GATES PRODUCTIVOS OPEN |
| `14_ADA_OPERATIONAL_SESSION_AND_WARMUP.md` | Nuevo contrato de consumo ADA: sesión individual Cosmos, warmup exclusivo de dos catálogos. | **DECIDED DESIGN / PLANNED** |

## Evidencia Master CURRENT

- **001A / VERIFIED STATIC:** commit `73603eead9fddf855d375710425387db0883d78e`: tooling ADA distribuido genera material ZIP AES-256 con verificador scrypt; el lector distingue `ABSENT/PRESENT/INVALID` y valida usuario de servicio, namespace y ambiente.
- **001B / VERIFIED STATIC:** commit `74f910737604beb38f3d604571e39055504106dd`: planner read-only para Navigation, Profiles, Tools, ADA Access, KPI Registry y KPI Definitions; Users consulta catálogo de snapshots aprobados y no es ejecutable.
- **001C / VERIFIED STATIC:** commit `e2177544754213f7ddcdee3045212a0971c8e8dd`: página separada `/master-projection`, rutas exactas independientes de Identity/Navigation, sesión Master de 900 s, CSRF, binding a fingerprint, planner inyectado; sin Apply ni Users REPLACE. Commit `94f26213ca28b550baf53d8ee34e34da7538ad17` corrige solo el test de herencia de entorno.
- **VERIFIED USER-REPORTED:** suite seleccionada 38/38 y regresiones anteriores 39/39; nuevas pruebas HTTP 3/3, Reader/Runtime 6/6, settings corregido 2/2. Son ejecuciones de distintos momentos, **no** una suite acumulable.
- **VERIFIED USER-REPORTED LOCAL (histórico 001C):** distribución `BUILT_UNQUALIFIED`, 67 wheels, init `SYNCED`, GET 200 tanto sin material como con material; el usuario confirmó login con material real generado. No equivale a calificación final desde HEAD limpio ni a producción.
- **001D.1/001D.2 — VERIFIED STATIC + USER-REPORTED TESTS:** executor individual de target exacto, prepare/confirm HTTP, CSRF, revalidación y verificación persistida; 50 pruebas seleccionadas PASS, Ruff y diff check PASS. Users REPLACE desde Master no se implementó.
- **001D.3 — VERIFIED USER-REPORTED DOCKER (`ca3ee508`):** distribución 67 wheels, material externo y recorrido Navigation publicado → Master Apply → Manager recargado proyectado/sincronizado; no extender el resultado a los otros cinco dominios.
- **001D.4 — VERIFIED STATIC + USER-REPORTED SYNC (`9c6daffd`):** Starter instalado como paquete en `project.py sync`; 24 pruebas seleccionadas PASS, 67 wheels, precheck y sincronización limpia sin `PYTHONPATH`; **image_build/runtime de esta nueva distribución UNVERIFIED**.

## Límites y conflictos todavía OPEN

- El gate histórico específico 001C de logout/relogin y la seguridad multiworker no se acreditan por las pruebas seleccionadas posteriores. Su historial de fallo de settings y corrección aislada se conserva como evidencia histórica, no como estado de 001D.
- `projection.apply` **sí está implementado** para seis proyecciones ordinarias desde 001D.1/001D.2; `users.replace` sigue declarado en el material pero **no ejecutable** desde Master.
- La excepción Master con usuario/contraseña de servicio frente a baseline histórica pre-Manager/Entra requiere conciliación contractual formal antes de habilitar producción; no reinterpreta los permisos del Manager.
- El material se consume desde una ruta externa opcional. La subida/warmup automático, rotación operacional productiva, Entra/Azure y CI general siguen **UNVERIFIED**. La distribución `9c6daffd` cuenta con sync local limpio, pero **no** tiene calificación Docker propia.
- Conservar los límites previos de Users Recovery, Resource Preparation y Home; Master no acredita esos otros frentes.

## Continuidad del checkpoint Master anterior (HISTORICAL para ADA Datos operacionales)

**PROPOSED / PRÓXIMO FOCO TÉCNICO ÚNICO:** calificar en Docker la distribución Master `9c6daffd` sin transferir la evidencia Docker de `ca3ee508`. El E2E de los otros cinco dominios, Users Master REPLACE, Azure/Entra y la migración Python conservan gates independientes. **PLANNED / SEPARATE:** ADA Operational Identification (Asignaciones/Cargos), descrito en `../10_MANAGER/10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md`; no ampliar automáticamente las seis proyecciones de Master.

---

## Actualización focal ADA Datos operacionales — 2026-09-29

**SUPERSEDED solo para el dominio operacional:** la última oración anterior que califica ADA Operational Identification como «PLANNED» es histórica del corte Master. `atlanticus:main@caced5d7711cf059d36ec61aecc9b3e9629bd41f` ya contiene su backend y Manager; su nuevo diseño de pestañas y el warmup continúan PLANNED.

Consultar `14_ADA_OPERATIONAL_SESSION_AND_WARMUP.md` para la distinción **DECIDED** entre consulta individual de sesión y warmup exclusivo de **Profiles + catálogo operacional**. Usuarios, promociones y asignaciones individuales están fuera de warmup. La composición productiva Entra y el scheduler de refresco permanecen UNVERIFIED / PLANNED. El contrato durable y el roadmap se encuentran en `../10_MANAGER/10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md` y `../10_MANAGER/11_ADA_OPERATIONAL_DATA_ROADMAP.md`.
