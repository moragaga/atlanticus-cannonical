# Artifact and Distribution Boundary

Estado: **CURRENT / HISTORICAL STARTER QUALIFICATION + ADA RESOURCE PREPARATION 001 LOCAL QUALIFIED / MASTER ACCESS MATERIAL PLANNED**  
Fuente de implementación actual contrastada: `moragaga/atlanticus@da75752e87036b8318f38f8d405c55e8cb18717d`. Evidencia Docker y tests del artifact nuevo aportada el 2026-09-28; no trasladar automáticamente qualification histórica de otros SHAs a este.

## Frontera y ownership

```text
SOURCE → ARTIFACT → DISTRIBUTION INPUT
```

Atlanticus produce artifacts y su contrato de entrega. El pipeline corporativo, la infraestructura productiva y el despliegue pertenecen a DevOps/host. Los wheels internos y externos se mantienen identificables; no eliminar dependencias externas sin auditar closure/locks.

Generadores backend y `deployment/local/generate_compose.py` corresponden a artifacts de procesos; **no** son `compose full` ni `project.py` del Starter Web. No fusionar ambos workflows.

## Starter Web y qualification histórica

`tooling/distribution/web/generate_starter.py` crea Starters editables Generic/ADA con manifest; `build_wheelhouse.py` y los scripts de qualify tratan closure e instalación portable. `distribution/` es resultado generado. Qualification anterior reportada: SOURCE_SMOKE/PORTABLE Generic+ADA con Python 3.14.2; número de wheels y tests antiguos corresponden a sus respectivos checkpoints, no al nuevo artifact.

Starter ADA incluye Gunicorn por worker y Compose `infra.yaml`, `web.yaml`, `full.yaml`, además de `tooling/project.py`. Los modos `local`/`durable` de Manager no autorizan inferir combinaciones no documentadas de providers. La configuración `production.py` exige identidad apropiada proporcionada por el host; el Starter no fabrica Entra ni incorpora secretos productivos.

## Resource Preparation 001 — CURRENT EN IMPLEMENTACIÓN / LOCAL VALIDATED

Commit `da75752` incorpora cambios acotados en:

```text
scopes/ada/web/application/ada-generic-application/
  src/ada/web/application/generic/{manager_deployment.py,resource_preparation.py}
  commented/ada/web/application/generic/{manager_deployment.py,resource_preparation.py}
  tests/{test_manager_deployment.py,test_resource_preparation_increment.py}

tooling/distribution/web/starter/ada/
  src/application/local_resources.py
  commented/application/local_resources.py
  deployment/compose/full.yaml
  commented/deployment/compose/full.yaml

tooling/tests/distribution/web/ada/
  {test_compose_integration.py,test_resource_job_degradation.py}
```

El job local espera Cosmos `/ready` y Azurite; prepara contenedor Blob/base Cosmos/seis contenedores. La Web de `full.yaml` no depende del éxito del job. El CLI actual es `ada-generic-manager-resources prepare|validate`, sin el viejo alias `ensure-local`. En producción se omite **toda** interacción Blob durante preparación; la base Cosmos existe externamente.

**VERIFIED USER-REPORTED:** suite ADA Generic + tooling (~295 pruebas por contadores aportados) y Ruff de los archivos acotados PASS, `git diff --check` PASS antes del commit. Nuevo Starter generado con 67 wheels internos, `BUILT_UNQUALIFIED`, `PRECHECK_PASS` y build Docker de la imagen `ada-generic:resource-validation-001`. El precheck aislado no sustituye el build y pruebas de runtime; los siguientes ensayos sí ejecutaron la imagen nueva.

**VERIFIED DOCKER EN ARTIFACT NUEVO:** ocho recursos `CREATED` en volúmenes nuevos; después `READY` y salida 0; comprobada idempotencia en recursos existentes, reinicio de Cosmos/Azurite manteniendo topología, diagnóstico parcial con Cosmos detenido y restauración de `READY` sin reiniciar la Web ya iniciada. Consola de errores estructurada y salidas 1/2 según el fallo observado. El artifact ensayado precede a la publicación del commit; repetir sobre HEAD limpio permanece **UNVERIFIED**.

**Límite Web:** un cold start con Cosmos ya detenido inició Gunicorn pero `/health/live` no respondió antes de diez segundos. Tras recuperar Cosmos, el mismo contenedor respondió 200 y `/health/ready` informó `checks: {}`. No declarar Home resiliente o readiness de dependencias por ese resultado.

## Evidencia de versiones anteriores — conservar como HISTORICAL

En el checkpoint previo de Starter Web se reportaron SOURCE_SMOKE y PORTABLE de Generic/ADA bajo Python 3.14.2, wheelhouses históricos Generic **36** / ADA **108**, y `33 passed` en la batería Compose/`project.py` específica de aquella versión. Más adelante existió un patch de etiquetas de provider con tests seleccionados aprobados y verificación visual de artifact aún pendiente en aquel corte. Estos antecedentes **no** sustituyen los **67 wheels** ni las pruebas Docker del nuevo incremento 001 y no autorizan mezclar rutas de distribución/backend.

## Master Projection — próximo foco aislado, sin código acreditado

Los servicios actuales de Users Recovery y su UI **dentro del Manager** ya existen y tienen validación histórica. Master es **otra página por URL**, independiente del Manager, para preparar Sources/proyecciones aun cuando el ambiente no tenga promovidos/Access. Debe manejar ausencia de material protegido (mensaje controlado y ninguna acción) o material válido/autenticación de servicio (plan y proyección autorizada mediante servicios existentes). Users utiliza snapshot aprobado, no promociona candidatos por inferencia.

El equipo debe poder preparar material protegido mediante **tooling ADA existente**, subirlo/integrarlo con warmup y utilizarlo sin consumo automático en el primer uso; el diseño exacto de generador, formato, loader, custodia, credenciales, rotación y autorización sigue OPEN. No agregar ahora generadores arbitrarios, variables inventadas, bypass de Manager ni adaptadores legacy. Contrato anterior pre-Manager/Entra necesita reconciliación explícita.

## `.env`, Python y seguridad

`*.env.detail` explica parámetros sin secretos. Mappings DEV/UAT/PRD y resolución productiva de secretos necesitan qualification propia. Baseline Project: Python 3.14.7, imagen objetivo `python:3.14.7-slim-trixie`. Implementación comprobada: metadata de ADA Generic `requires-python ==3.14.2`, tooling Web `PYTHON_VERSION='3.14.2'` e imagen Web histórica 3.14.2/slim-bookworm; **OPEN / SEPARATE**, no actualizar silenciosamente en Master.

## Gates

| Frontera | Estado |
|---|---|
| SOURCE_SMOKE / PORTABLE históricos | CLOSED / evidencia de sus respectivos checkpoints |
| ADA Compose `full`, artifact Resource Preparation 001 y pruebas de creación/fallo local | CLOSED / VERIFIED USER-REPORTED EN ALCANCE |
| Docker cold start con Cosmos ya detenido y Home real | OPEN / NO PASA ventana probada de 10 s |
| Snapshot de documentos tras reinicio, Sources/projections y KPI browser E2E | UNVERIFIED |
| Azure real, Entra productiva, telemetría externa, CI global | UNVERIFIED |
| Archivo Master generado/consumido por tooling/warmup y página externa | PLANNED / CONTRATO OPEN |
| Usuarios aprobados vía Users Recovery dentro del Manager | CURRENT / LAB VALIDATED, no Master implementada |

No ampliar Resource Preparation para absorber Master ni la recuperación visual del Home. El siguiente chat primero inspeccionará herramientas y puertos reales de Master, y después congelará diseño antes de código.
