# Distribution and Tooling — Index

Estado: **CURRENT HISTORICAL QUALIFICATION + MASTER ACCESS MATERIAL NEXT (PLANNED)**  
Corte de inspección: `atlanticus@208c8d6244795ba92cbe6f8e6b11e9743191367d`. Mantener evidencias previas de wheelhouse/PORTABLE/Compose asociadas a sus commits originales; no trasladarlas automáticamente a este HEAD.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_BACKEND_GENERATION.md` | Generación de artifacts de procesos backend. | CURRENT / OTHER FOCUS |
| `02_FRONTEND_GENERATION.md` | Starter Web Generic/ADA, wheelhouse y qualification. | CURRENT + HISTORICAL EVIDENCE |
| `03_ARTIFACT_DISTRIBUTION.md` | Estado Starter/ADA, Gunicorn/Compose y futuras extensiones. | CURRENT DIRECTION |
| `04_SCRIPTS_VALIDATION.md` | Gates de integridad por frontera. | CURRENT DIRECTION |
| `05_SUPPORT_SERVICES.md` | Cosmos/Storage local/productivo. | CURRENT DIRECTION |
| `06_ENV_DETAIL.md` | Variables explicadas, sin secretos. | CURRENT DIRECTION |
| `07_READMES.md` | README sólo al cierre de entregable. | CURRENT POLICY |
| `08_LOADERS.md` | Contratos de loaders de aplicación. | PRODUCT REQUIREMENT |
| `09_SOURCE_LEDGER.md` | Evidencia histórica previa. | HISTORICAL |

## Tooling Web existente — referencia para el siguiente hito

La inspección del repositorio confirma la existencia de:

```text
tooling/distribution/web/generate_starter.py
tooling/distribution/web/ada/build_distribution.py
tooling/distribution/web/ada/qualify_distribution.py
tooling/distribution/web/starter/ada/tooling/project.py
tooling/distribution/web/starter/ada/
tooling/distribution/web/starter/base/
tooling/tests/distribution/web/
```

Estos son **puntos a revisar**, no evidencia de un generador Master/warmup implementado. `deployment/local/generate_compose.py` conserva otra responsabilidad y no debe confundirse con el generador del Starter Web.

## Próximo requisito acotado

Master Projection requiere material protegido de acceso creado mediante el tooling **ya existente** de la distribución ADA y entregado/integrado en el mecanismo de warmup por acordar. La página externa debe distinguir archivo ausente (informar, no autenticar ni proyectar) y material válido (autenticación de servicio, estado/plan de proyecciones y acciones autorizadas). **PLANNED / CONTRACT OPEN**: nombre, contenido, custodia, cifrado/verificador, ubicación, distribución y lectura en warmup. No inventar `env` redundantes, rutas físicas configurables sin necesidad, secretos en Starter ni adaptadores legacy.

Los cierres previos de Starter Compose local y pruebas de tooling permanecen históricos. La falta de calificación productiva Entra/Azure, CI general, reinicio E2E y normalización Python sigue **UNVERIFIED/OTHER FOCUS**. No extender el siguiente hito a refactors de tooling general; integrar sólo la frontera necesaria para Master tras acuerdo de contrato.
