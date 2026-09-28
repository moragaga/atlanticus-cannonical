# Distribution and Tooling — Index

Estado: **CURRENT / WEB ADA COMPOSE FULL LOCAL VERIFIED / NEXT OUTSIDE TOOLING**

Checkpoint estático de referencia: `moragaga/atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`. Las qualification anteriores de wheelhouse/PORTABLE y las pruebas recientes de Compose tienen fuentes/commits diferentes. No reunirlos en un único gate de HEAD.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_BACKEND_GENERATION.md` | Process artifact generation y sus contratos. | CURRENT / SEPARATE |
| `02_FRONTEND_GENERATION.md` | Web Generic/ADA editable, wheelhouse, qualification. | CURRENT; algunos datos de evidencia históricos |
| `03_ARTIFACT_DISTRIBUTION.md` | Estado actualizado Starter ADA, Gunicorn/Compose, límites productivos y futura página aislada. | CURRENT / UPDATED |
| `04_SCRIPTS_VALIDATION.md` | Gates y validaciones de tooling. | CURRENT DIRECTION |
| `05_SUPPORT_SERVICES.md` | Requisitos locales/productivos Cosmos/Storage. | CURRENT DIRECTION; Compose ADA ya materializado |
| `06_ENV_DETAIL.md` | Variables documentales sin secretos. | CURRENT DIRECTION / COMPLETENESS UNVERIFIED |
| `07_READMES.md` | Política README sólo al cierre de entregable. | CURRENT DIRECTION |
| `08_LOADERS.md` | Contratos de loaders de aplicación. | PRODUCT REQUIREMENT |
| `09_SOURCE_LEDGER.md` | Evidencia histórica de Starter/Manager previa. | HISTORICAL; delta reciente en `03_ARTIFACT_DISTRIBUTION.md` |

## Nuevo checkpoint acotado

- **CLOSED / VERIFIED REPORTADO:** pruebas Compose ADA `33 passed`, Starter generado, artifact construido, imagen `full` generada e infraestructura local Cosmos/Azurite inicializada; Web levantó tres workers Gunicorn y el usuario confirmó funcionamiento.
- **CURRENT / TESTS VERIFIED REPORTADOS:** patch de labels visuales del Manager (Source/Projection → Local/Blob/Cosmos). Validación visual desde artifact nuevo aún UNVERIFIED.
- **OPEN / SEPARATE:** persistencia después de `down/up` y consumo completo sin reproyección, host productivo Entra, pipeline Azure, tests globales y migración Python objetivo 3.14.7.
- **PLANNED:** herramienta/material protegido y página aislada de proyección. No crear aquí una nueva implementación antes de que Users special recovery tenga contrato.

`deployment/local/generate_compose.py` **no** es el generador de Compose del Starter Web. Conservar separación de ownership y no introducir adapters legacy. La nueva prioridad del Project es backend Users recovery, no otra ronda de tooling.
