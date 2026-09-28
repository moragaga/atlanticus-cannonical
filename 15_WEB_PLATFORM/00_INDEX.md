# Atlanticus Web Platform — Canonical Index

Estado: **CURRENT / USERS RECOVERY CLOSED EN ALCANCE HISTÓRICO / RESOURCE PREPARATION 001 LOCAL CLOSED / MASTER 001A+001B CLOSED / MASTER 001C IMPLEMENTED, QUALIFICATION IN PROGRESS / 001D PLANNED (SOLO DISEÑO)**  
Corte específico Master contrastado: `moragaga/atlanticus@94f26213ca28b550baf53d8ee34e34da7538ad17`; canonical anterior `moragaga/atlanticus-cannonical@ed1edd7a04533cf32a2b32844cb79b9edbe7bcc8`; decisiones históricas `moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Este corte **no** revalida otros frentes del Project ni los DOCX históricos completos de identidad.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CAPABILITY_INDEPENDENCE.md` | Ownership genérico de capabilities Web. | CURRENT / NO MODIFICADO |
| `02_USER_ACTIVITY_HISTORY.md` | Dirección histórica de actividad/sesión. | CURRENT DIRECTION / NO MODIFICADO |
| `03_RESOURCE_PROVISIONING.md` | Resource Preparation 001 y qualification local parcial. | CURRENT / LOCAL SCOPE CLOSED / NO REABIERTO |
| `04_WEB_READINESS_AND_DECOUPLING.md` | Estado Tool, collector y Home degradado. | CURRENT + OPEN DEFERRED |
| `05_DEPLOYMENT_ORDER.md` | Web/recursos/proyección/backend; Cloud E2E pendiente. | CURRENT DIRECTION |
| `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md` | Master 001A material, 001B planner y 001C página protegida read-only. | CURRENT IMPLEMENTED + QUALIFICATION GAPS |
| `07_PROJECTION_ORCHESTRATION.md` | Targets exactos y seis entradas del planner; Users especial; Apply no implementado. | CURRENT PREVIEW + 001D PLANNED |
| `08_EXTERNAL_RESOURCE_REQUIREMENTS.md` | Conexiones nombradas y ownership de recursos externos. | CONTRACT DESIGN / NO MODIFICADO |
| `09_CURRENT_GAPS.md` | Checkpoint histórico de Starter de un SHA anterior. | HISTORICAL / NO USAR COMO ESTADO MASTER |
| `10_SOURCE_LEDGER.md` | Historial y referencias de otros hitos Web. | HISTORICAL / NO REVALIDADO GLOBALMENTE |
| `11_OPEN_ITEMS.md` | Qualification pendiente 001C, conflicto contractual e inicio acotado 001D. | CURRENT |
| `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` | Ownership genérico de Users/Profiles/Access/Navigation. | CURRENT BASELINE |
| `13_USERS_PROJECTION_RECOVERY.md` | Snapshots aprobados y recuperación especial dentro del Manager. | CURRENT / SCOPE CLOSED + GATES PRODUCTIVOS OPEN |

## Evidencia Master CURRENT

- **001A / VERIFIED STATIC:** commit `73603eead9fddf855d375710425387db0883d78e`: tooling ADA distribuido genera material ZIP AES-256 con verificador scrypt; el lector distingue `ABSENT/PRESENT/INVALID` y valida usuario de servicio, namespace y ambiente.
- **001B / VERIFIED STATIC:** commit `74f910737604beb38f3d604571e39055504106dd`: planner read-only para Navigation, Profiles, Tools, ADA Access, KPI Registry y KPI Definitions; Users consulta catálogo de snapshots aprobados y no es ejecutable.
- **001C / VERIFIED STATIC:** commit `e2177544754213f7ddcdee3045212a0971c8e8dd`: página separada `/master-projection`, rutas exactas independientes de Identity/Navigation, sesión Master de 900 s, CSRF, binding a fingerprint, planner inyectado; sin Apply ni Users REPLACE. Commit `94f26213ca28b550baf53d8ee34e34da7538ad17` corrige solo el test de herencia de entorno.
- **VERIFIED USER-REPORTED:** suite seleccionada 38/38 y regresiones anteriores 39/39; nuevas pruebas HTTP 3/3, Reader/Runtime 6/6, settings corregido 2/2. Son ejecuciones de distintos momentos, **no** una suite acumulable.
- **VERIFIED USER-REPORTED LOCAL:** distribución `BUILT_UNQUALIFIED`, 67 wheels, init `SYNCED`, GET 200 tanto sin material como con material; el usuario confirmó login con material real generado. No equivale a calificación final desde HEAD limpio ni a producción.

## Límites y conflictos todavía OPEN

- 001C **no está completamente calificado**: falta rerun conjunto posterior a `94f2621` y comprobación manual expresa de plan visible y logout/relogin. El último runner combinado falló 1 test de settings por variable exportada; el correctivo aislado pasó 2/2.
- `projection.apply` y `users.replace` están **declarados** como operaciones posibles del formato de 001A, pero no implementados en la UI/HTTP de 001C.
- La excepción Master con usuario/contraseña de servicio frente a baseline histórica pre-Manager/Entra requiere conciliación contractual formal antes de habilitar producción; no reinterpreta los permisos del Manager.
- El material se consume desde una ruta externa opcional. La subida/warmup automático, rotación operacional productiva, Entra/Azure, CI general y prueba Docker sobre checkout limpio son **UNVERIFIED**.
- Conservar los límites previos de Users Recovery, Resource Preparation y Home; Master no acredita esos otros frentes.

## Único foco siguiente

`MASTER-PROJECTION-001D`: **inspección y diseño del contrato para aplicar exclusivamente las seis proyecciones ordinarias reutilizando servicios reales**. Sin código ni Git en la etapa inicial. Antes de cualquier nueva implementación, registrar los tres gates pendientes de 001C y contrastar decisiones pre-Manager. Users REPLACE, integración warmup, vínculo organizacional ADA, Home, KPI y alarmas pertenecen a incrementos separados.
