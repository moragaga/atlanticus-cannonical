# Atlanticus Web Platform — Canonical Index

Estado: **CURRENT / USERS RECOVERY NEXT / EXTERNAL PROJECTION + ADA USER DOMAIN PLANNED**

Inspección estática para el cierre: `moragaga/atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`. No sustituye las evidencias históricas específicas de cada capability.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CAPABILITY_INDEPENDENCE.md` | Ownership de capabilities genéricas Web. | CURRENT |
| `02_USER_ACTIVITY_HISTORY.md` | Dirección histórica de activity/session. | CURRENT DIRECTION |
| `03_RESOURCE_PROVISIONING.md` | Recursos físicos y plan de Manager parcial. | CURRENT / PLAN GLOBAL SEPARATE |
| `04_WEB_READINESS_AND_DECOUPLING.md` | Arranque resiliente y degradación independiente. | CURRENT |
| `05_DEPLOYMENT_ORDER.md` | Orden de despliegue por requisitos. | DIRECTION; revalidar al construir página aislada |
| `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md` | Página de proyección aislada (fuera de Manager), acceso protegido y refinamiento de baseline anterior. | PLANNED / UPDATED |
| `07_PROJECTION_ORCHESTRATION.md` | Targets exactos y futura excepción administrativa Users. | CURRENT + PLANNED |
| `08_EXTERNAL_RESOURCE_REQUIREMENTS.md` | Requisitos de recursos externos. | CONTRACT DESIGN |
| `09_CURRENT_GAPS.md` | Gap ledger previo de Starter; su clasificación Compose anterior queda SUPERSEDED por nuevo delta. | HISTORICAL CHECKPOINT |
| `10_SOURCE_LEDGER.md` | Evidencia histórica anterior. | HISTORICAL |
| `11_OPEN_ITEMS.md` | Abiertos vigentes y único siguiente foco. | CURRENT / UPDATED |
| `12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` | Contracts CURRENT de Users/Profiles/Access/Navigation. | CURRENT |
| `13_USERS_PROJECTION_RECOVERY.md` | Contrato a debatir: aprobados en Storage, validar y reconciliar Cosmos. | PLANNED / NEW DOCUMENT |

## Fronteras de este cierre

La implementación durable de Users conserva Blob Registry y Cosmos Users Runtime; no hay reconciliación integral. La nueva página independiente debe poder operar sin Users/Profiles/Access previamente proyectados, pero no concede Manager access. La información de área/cargo/grupo es ADA-specific y no concede permisos. `extra` sólo en Cosmos es una idea futura y no se implementa ahora.

En Starter ADA se verificó manualmente el arranque local Compose `full` (Cosmos vNext/Azurite/recursos/Gunicorn), más **33 tests específicos** reportados. No equiparar ese resultado con reinicio E2E acreditado, Entra productiva, auditoría completa de autorización ni CI general. El patch visual de etiquetas tiene pruebas reportadas, sin validación visual final del artifact regenerado.

**NEXT único:** `USERS-PROJECTION-RECOVERY-001` sobre contratos actuales; después página aislada y finalmente datos operacionales ADA como incrementos separados.
