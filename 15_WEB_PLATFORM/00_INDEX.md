# Atlanticus Web Platform — Canonical Index

Estado: **CURRENT / USERS RECOVERY VALIDATED SCOPE CLOSED / ADA RESOURCE PREPARATION 001 LOCAL CLOSED / MASTER PROJECTION NEXT**  
Corte de código: `moragaga/atlanticus@da75752e87036b8318f38f8d405c55e8cb18717d`; decisiones consultadas `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Base documental contrastada: `atlanticus-cannonical@e713c1485f63aeb644b32ddd14ce62b28ede0411` **antes** de integrar estos reemplazos. Qualification Docker de recursos corresponde al artifact generado del incremento; no es CI sobre un checkout limpio del HEAD.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CAPABILITY_INDEPENDENCE.md` | Ownership genérico de capabilities Web. | CURRENT |
| `02_USER_ACTIVITY_HISTORY.md` | Dirección histórica de actividad/sesión. | CURRENT DIRECTION |
| `03_RESOURCE_PROVISIONING.md` | ADA Resource Preparation 001: implementación y calificación local parcial; inventario global separado. | CURRENT / LOCAL SCOPE CLOSED |
| `04_WEB_READINESS_AND_DECOUPLING.md` | Estados Tool, collector, evidencia de recuperación y límites de Home degradado. | CURRENT + OPEN DEFERRED |
| `05_DEPLOYMENT_ORDER.md` | Despliegue Web/recursos/proyección/Backend; Compose local ya ensayado; Cloud E2E pendiente. | CURRENT + DIRECTION |
| `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md` | Próxima página **Master Projection externa**; dos estados del material de acceso generado por tooling. | PLANNED / NEXT |
| `07_PROJECTION_ORCHESTRATION.md` | Dependencias exactas; Users especial CURRENT y uso posterior de servicios actuales en Master. | CURRENT + PLANNED |
| `08_EXTERNAL_RESOURCE_REQUIREMENTS.md` | Conexiones nombradas y ownership de recursos externos. | CONTRACT DESIGN |
| `09_CURRENT_GAPS.md` | Checkpoint histórico de Starter anterior. | HISTORICAL / NO CURRENT LEDGER |
| `10_SOURCE_LEDGER.md` | Referencias y pruebas históricas previas. | HISTORICAL |
| `11_OPEN_ITEMS.md` | Pendientes, seguridad y siguiente foco único. | CURRENT |
| `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` | Baseline de ownership Users/Profiles/Access/Navigation; delta Users Projection en 13. | CURRENT BASELINE |
| `13_USERS_PROJECTION_RECOVERY.md` | Snapshots aprobados, RESTORE/REPLACE y UI del Manager con evidencia/limitaciones. | CURRENT / VALIDATED SCOPE CLOSED |

## Estado CURRENT / VERIFIED

- Blob Registry y snapshots aprobados de Users son authority durable; Cosmos contiene promovidos/runtime. La UI interna del Manager de Users Projection ya existe. Es diferente de la próxima página Master, externa al Manager.
- El commit `da75752` añade `resource_preparation.py`, adapta `manager_deployment.py`, el job Compose `local_resources.py`, Compose `full.yaml` y pruebas. El nuevo comando es `prepare|validate`; producción omite Blob totalmente. En Docker aislado el usuario obtuvo ocho `CREATED` iniciales, luego ocho `READY`; verificó reinicios, idempotencia, error parcial y recuperación de recursos.
- `full.yaml` inicia Web sin dependencia del éxito del job `resources`. Esa decisión de despliegue no demuestra que el Home sea inmediatamente usable cuando Cosmos ya está detenido al arrancar.

## Límites demostrados / OPEN

- En un arranque Web nuevo con Cosmos detenido, `/health/live` agotó diez segundos. Después de recuperar Cosmos, la Web respondió `/health/live` y `/health/ready` HTTP 200, pero readiness reportó `checks: {}`. No usar esto como prueba de Home o dependencias funcionales.
- El collector conserva último dato válido y continúa intentando tras errores por contrato de código y unit tests, pero su recuperación y entrega a un Home real tras corte Cosmos quedan **UNVERIFIED E2E**.
- Crear contenedores en emuladores no prueba Azure real, documentos de negocio persistidos, proyecciones/publicaciones ni creación automática de componentes desde Tool.

## Único foco siguiente

**`MASTER-PROJECTION-001`: debate/contrato, no generación de código inmediata.** Revisar tooling/distribución/warmup, loaders, identidad excepcional y servicios de proyección implementados. Dos flujos obligatorios: material ausente → página controlada, sin acceso; material íntegro presente + credenciales verificadas → inspección y despliegue autorizado de proyecciones pertinentes, con Users especial desde snapshot aprobado. La tensión con baseline Entra y el destino Users vacío permanecen OPEN y deben resolverse antes de implementar. No mezclar cargo/área/grupo ADA, Home, componentes dinámicos, alarmas ni productización general.
