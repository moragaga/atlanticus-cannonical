# Distribution and Tooling — Index

Estado: **CURRENT HISTORICAL QUALIFICATION + RESOURCE PREPARATION 001 LOCAL VALIDATED + MASTER ACCESS MATERIAL NEXT (PLANNED)**  
Corte de implementación contrastado: `atlanticus@da75752e87036b8318f38f8d405c55e8cb18717d`. Mantener separadas evidencias históricas de wheelhouse/PORTABLE/Compose, pruebas recientes del artifact `ada-generic:resource-validation-001` y calificación pendiente del commit en checkout limpio.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_BACKEND_GENERATION.md` | Artifacts de procesos backend. | CURRENT / OTHER FOCUS |
| `02_FRONTEND_GENERATION.md` | Starter Web Generic/ADA, wheelhouse y qualification. | CURRENT + HISTORICAL EVIDENCE |
| `03_ARTIFACT_DISTRIBUTION.md` | Starter/ADA, Gunicorn/Compose y validación local Resource Preparation 001. | CURRENT + LOCAL SCOPE CLOSED |
| `04_SCRIPTS_VALIDATION.md` | Gates por frontera. | CURRENT DIRECTION |
| `05_SUPPORT_SERVICES.md` | Cosmos/Storage local y productivo; contratos generales. | CURRENT DIRECTION |
| `06_ENV_DETAIL.md` | Variables explicadas sin secretos. | CURRENT DIRECTION |
| `07_READMES.md` | README sólo al cierre final del entregable. | CURRENT POLICY |
| `08_LOADERS.md` | Contratos de loaders; inspección requerida para Master. | PRODUCT REQUIREMENT |
| `09_SOURCE_LEDGER.md` | Evidencia de generaciones anteriores. | HISTORICAL |

## Fuente real a inspeccionar en el siguiente hito

```text
tooling/distribution/web/generate_starter.py
tooling/distribution/web/ada/build_distribution.py
tooling/distribution/web/ada/qualify_distribution.py
tooling/distribution/web/starter/ada/tooling/project.py
tooling/distribution/web/starter/ada/src/application/{wsgi.py,runtime.py,production.py}
tooling/distribution/web/starter/{ada,base}/
```

Estos son puntos de integración existentes, **no evidencia** de un generador de material Master/warmup ni contrato de secrets ya implementados. No confundir `deployment/local/generate_compose.py` con el Starter Web.

## Resource Preparation 001 — cierre acotado

El código `da75752` incluye job local, CLI `prepare|validate`, resultados por recurso y `full.yaml` con Web independiente del éxito de `resources`. Artifact nuevo con 67 wheels y precheck PASS, Docker con volúmenes nuevos (`CREATED` ×8), idempotencia, persistencia de topología en reinicio, falla parcial Cosmos y recuperación son **VERIFIED USER-REPORTED LOCAL**. No implica publicación/proyección, Azure productiva ni cold start Web completamente operativo cuando Cosmos está caído.

## Master: próxima frontera contractual única

Página externa de proyección cuando aún no existen los perfiles/usuarios del Manager. Requiere material protegido generado con tooling existente e integrado en warmup, cuya **ausencia** produzca página informativa sin acciones y cuya **presencia válida** habilite autenticación de servicio y proyecciones autorizadas. El formato, loader, distribución, seguridad y propietario del material están OPEN. Primero contrastar código/decisions/canonical; segundo acordar contrato; sólo después autorizar incrementos de código. No mezclar datos organizacionales ADA, Home, alarmas, KPI ni migración Python.
