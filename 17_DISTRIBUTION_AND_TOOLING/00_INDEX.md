# Distribution and Tooling — Index

Estado: **CURRENT HISTORICAL QUALIFICATION + RESOURCE PREPARATION 001 LOCAL CLOSED + MASTER ACCESS MATERIAL 001A CURRENT + MASTER WEB PREVIEW 001C LOCAL TESTED; MASTER APPLY PLANNED**  
Master contrastado en `atlanticus@94f26213ca28b550baf53d8ee34e34da7538ad17`; decisiones históricas `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Preservar evidencias históricas de wheelhouse/PORTABLE/Compose y no transferir las pruebas de un build sobre working tree a la qualification del HEAD publicado.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_BACKEND_GENERATION.md` | Artifacts de procesos backend. | CURRENT / OTHER FOCUS |
| `02_FRONTEND_GENERATION.md` | Starter Web Generic/ADA, wheelhouse y calificaciones por versión. | CURRENT + HISTORICAL EVIDENCE |
| `03_ARTIFACT_DISTRIBUTION.md` | Starter, Compose, Resource Preparation 001 y material/ensayo Master actuales. | CURRENT + QUALIFICATION GAPS |
| `04_SCRIPTS_VALIDATION.md` | Gates por frontera. | CURRENT DIRECTION |
| `05_SUPPORT_SERVICES.md` | Cosmos/Storage local y productivo; contratos generales. | CURRENT DIRECTION |
| `06_ENV_DETAIL.md` | Variables documentadas sin secretos; material Master por ruta externa opcional. | CURRENT DIRECTION + MASTER ACTUAL |
| `07_READMES.md` | README al cierre final del entregable. | CURRENT POLICY |
| `08_LOADERS.md` | Contratos generales de loaders; **warmup Master productivo sigue UNVERIFIED**. | REQUIREMENT / GATE |
| `09_SOURCE_LEDGER.md` | Evidencias históricas de tooling y distribución. | HISTORICAL; NO EQUIVALE A NUEVA QUALIFICATION |

## Implementación real Master distribuido

```text
tooling/distribution/web/generate_starter.py
tooling/distribution/web/ada/build_distribution.py
tooling/distribution/web/starter/ada/tooling/master_projection.py
tooling/distribution/web/starter/ada/src/application/master_projection/{material,reader}.py
tooling/distribution/web/starter/ada/src/application/runtime.py
scopes/ada/web/application/ada-generic-application/.env.detail
```

`tooling/master_projection.py` **no** es una ruta raíz del repositorio: el generador se incorpora al Starter ADA. Se invoca con Python; no se introducen permisos de ejecución o scripts legacy por esa observación. Genera material fuera de la distribución, solicita contraseña interactivamente y no incluye secretos en wheels.

`ADA_MASTER_PROJECTION_MATERIAL_PATH` es opcional y debe apuntar a un ZIP existente, absoluto, **externo**; el reader inspecciona AUSENTE/PRESENTE/INVÁLIDO. Esta ruta no es prueba de un uploader/warmup productivo: no inventar uno sin inspeccionar el host.

## Resource Preparation 001 — cierre conservado

El código `da75752e87036b8318f38f8d405c55e8cb18717d` incluye job local, CLI `prepare|validate`, informes independientes y `full.yaml` que inicia Web sin depender del éxito del job `resources`. Artifact histórico con 67 wheels, precheck, Docker con ocho recursos desde cero, idempotencia, reinicio, fallo parcial y recuperación son VERIFIED USER-REPORTED LOCAL. No implica proyección real, Azure, Home estable en cold start sin Cosmos ni qualification de toda la aplicación.

## Master 001A/B/C — nueva evidencia de este corte

**VERIFIED STATIC:** commits `73603ee` (material), `74f9107` (planner), `e217754` (HTTP) y `94f2621` (test de entorno). **VERIFIED USER-REPORTED:** test suites acotadas 38/38, 39/39, 3/3 integración HTTP, 6/6 reader/runtime y 2/2 settings corregido, ejecutadas en diferentes momentos. Distribución smoke v2 `BUILT_UNQUALIFIED` de 67 wheels, `SYNCED` y GET 200 con/sin material; login real informado.

**OPEN:** rerun conjunto post-`94f2621`, confirmación explícita de seis filas/estados y logout/relogin real, build/qualification desde checkout limpio de HEAD final, pipeline automático de material/warmup y seguridad productiva. El manifest smoke v2 muestra `source_git_head: efe231d...` aunque fue construido con modificaciones no commiteadas; no usar ese SHA como fingerprint del código exacto de la imagen.

## Único foco siguiente

`MASTER-PROJECTION-001D`: diseño del contrato para `projection.apply` de seis proyecciones ordinarias, backend primero y servicios actuales. **Sin código, nuevo empaquetado, warmup, Users REPLACE o refactor general durante el debate.** No confundir `deployment/local/generate_compose.py` (artifacts backend) con los flujos del Starter Web. La migración transversal Python `3.14.2` → `3.14.7`, `.env.detail` global, Azure y CI siguen otros incrementos.
