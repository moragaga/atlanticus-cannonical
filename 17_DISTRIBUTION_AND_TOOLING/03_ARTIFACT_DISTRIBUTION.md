# Artifact and Distribution Boundary

Estado: **CURRENT / HISTORICAL STARTER QUALIFICATION + RESOURCE PREPARATION 001 LOCAL CLOSED + MASTER 001A GENERATOR CURRENT / 001C SMOKE LOCAL PASS, BUILT_UNQUALIFIED**  
Resource Preparation: corte previo `atlanticus@da75752e87036b8318f38f8d405c55e8cb18717d`. Master: corte `atlanticus@94f26213ca28b550baf53d8ee34e34da7538ad17`. Conservar qualification atribuida a sus propias versiones; no inferir que el artifact smoke construido con working tree equivale bit a bit a HEAD limpio.

## Frontera y ownership

```text
SOURCE → ARTIFACT → DISTRIBUTION INPUT
```

Atlanticus produce artifacts y su contrato de entrega; DevOps/host es dueño del pipeline corporativo, infraestructura productiva y despliegue. Wheels internos y dependencias externas siguen identificables, sin borrar paquetes sin auditoría del closure/lock. Generación `deployment/local/generate_compose.py` del backend no es `compose full` ni `project.py` del Starter Web. No fusionar flujos.

## Starter Web y qualification histórica

`tooling/distribution/web/generate_starter.py` crea Starters editables Generic/ADA con manifest. `build_wheelhouse.py` y scripts de qualification tratan closure e instalación portable. `distribution/` es resultado generado. SOURCE_SMOKE/PORTABLE históricos Generic+ADA se reportaron con Python `3.14.2` y wheelhouses **36**/**108** de otros checkpoints; esas cifras no caracterizan la distribución Master de este hito.

Starter ADA conserva Gunicorn por worker y Compose `infra.yaml`, `web.yaml`, `full.yaml` más `tooling/project.py`. Los modos Manager local/durable no implican combinaciones adicionales de providers no documentadas. `production.py` necesita identidad productiva proporcionada por el host: Starter no fabrica Entra ni incorpora secretos productivos.

## Resource Preparation 001 — CURRENT / LOCAL VALIDATED, sin reabrir

Commit `da75752` contiene cambios acotados en:

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

El job local espera Cosmos `/ready` y Azurite; prepara contenedor Blob/base Cosmos/seis contenedores. Web en `full.yaml` no depende del éxito del job. El CLI vigente es `ada-generic-manager-resources prepare|validate`; no resucitar `ensure-local`. En producción se omite toda interacción Blob en esta preparación y la base Cosmos existe externamente.

**VERIFIED USER-REPORTED HISTÓRICO:** suite ADA Generic + tooling (~295 según contadores), Ruff acotado y `git diff --check` PASS antes del commit; Starter 67 wheels, `BUILT_UNQUALIFIED`, `PRECHECK_PASS` e imagen Docker `ada-generic:resource-validation-001`. En emuladores aislados se observaron ocho `CREATED`, luego ocho `READY` y salida 0; idempotencia, reinicio conservando topología, diagnóstico parcial con Cosmos detenido y recuperación a `READY` sin reiniciar la Web ya iniciada.

**Límite Web no cerrado:** cold start con Cosmos ya detenido inició Gunicorn, pero `/health/live` no respondió dentro de diez segundos. Una vez recuperado Cosmos el mismo contenedor devolvió HTTP 200; `/health/ready` indicó `checks: {}`. No certificar Home ni readiness de dependencias a partir de esos resultados.

## MASTER-PROJECTION-001A — tooling y contrato CURRENT

Código/commit auditados:

```text
73603eead9fddf855d375710425387db0883d78e
tooling/distribution/web/starter/ada/tooling/master_projection.py
tooling/distribution/web/starter/ada/src/application/master_projection/material.py
```

El generador, distribuido **dentro del Starter ADA**, utiliza prompts interactivos de usuario y contraseña de servicio y genera un **ZIP AES-256** con verificador `scrypt`. El ZIP vincula servicio, application namespace y ambiente, lista `projection.preview`, `projection.apply`, `users.replace` como acciones declaradas y requiere protección/custodia externa. **Solo preview Web** está implementado actualmente. El generador no se invoca como `./tooling/master_projection.py` por carecer de bit ejecutable; ejecutarlo mediante Python, sin crear un wrapper/alias legacy. No existe la ruta `tooling/master_projection.py` en la raíz del repo auditado.

El script rechaza generar el material dentro del proyecto distribuido o sobrescribir un archivo existente; nunca incluir contraseñas/ZIP en Git, wheelhouse, `.env.detail` ni Starter. Pérdida de contraseña requiere generar material nuevo; su primer uso no lo consume.

## MASTER-PROJECTION-001B/C — planner, reader y runtime CURRENT

`74f9107` introduce planner read-only de los seis pares existentes. `e217754` integra página Master independiente, reader del Starter, configuración y registro de middleware exacto antes de Identity/Navigation; `94f2621` aísla test de configuración frente a variable heredada. Variable implementada:

```text
ADA_MASTER_PROJECTION_MATERIAL_PATH  # ruta opcional externa absoluta; NO almacena secreto
```

Al arrancar, `StarterMasterMaterialReader` informa `ABSENT/PRESENT/INVALID`. **ABSENT:** página informativa, cero formulario/operaciones. **INVALID:** error controlado. **PRESENT:** login Master independiente; tras autorización muestra plan exclusivamente de lectura. Su excepción Identity solo cubre `/master-projection` y `/master-projection/logout`, no todo el prefijo. El runtime usa `create_worker_runtime()` y conserva recursos durables hasta cierre del worker.

No afirmar que esta configuración implemente un *warmup/upload* de material productivo. El lector de ruta externa está probado localmente; integración Azure/Key Vault, custodia, rotación y política de exención Entra pre-Manager requieren contrato/verificación aparte.

## Qualification específica Master — 2026-09-28

- **VERIFIED USER-REPORTED:** 38/38 selección 001C, 39/39 regresiones de corte anterior; 3/3 HTTP integrado después del correctivo Navigation, 6/6 Reader/Runtime y 2/2 settings tras el correctivo final. Ejecuciones solapadas/de versiones intermedias: no sumar ni atribuir todas al mismo HEAD.
- **Smoke artifact v1:** 67 wheels, `BUILT_UNQUALIFIED`, primer intento `/master-projection` falló HTTP 500 por `AccessContextError` de Navigation al exigir snapshot Identity que la ruta Master excluía. **SUPERSEDED por correctivo de orden de middleware** y pruebas de integración; no borrar este hallazgo de la trazabilidad.
- **Smoke artifact v2:** 67 wheels internos, `BUILT_UNQUALIFIED`, `SYNCED`, GET `/master-projection` HTTP 200 sin material y con material real externo. El usuario generó un ZIP nuevo y confirmó login con usuario de servicio. El manifest anuncia `source_git_head: efe231d61c9d5a6f4eca1e3f22a201a9b3c1861b`; fue construido **antes** de registrar el commit 001C desde working tree modificado. No usar ese SHA como prueba de identidad del código efectivamente empaquetado ni declarar PORTABLE/production qualification final.
- **Constructor:** ejecutar con Python 3.14.2 y librería `packaging` disponible; el primer intento sin ella produjo `ModuleNotFoundError`. Se utilizó `uv run --no-project --python 3.14.2 --with packaging==25.0 ...` sin introducir dependencias runtime por un fallo aislado del constructor.
- **UNVERIFIED:** rerun conjunto posterior al correctivo del test en `94f2621`; inventario y logout/relogin expresamente confirmados en browser; qualify completo del artifact construido desde checkout limpio HEAD; múltiples workers reales, Azure/Entra productivos y warmup automático.

## Evidencia histórica preservada

En versiones anteriores se reportaron SOURCE_SMOKE/PORTABLE offline para los dos perfiles, ruedas Generic 36/ADA 108 y `33 passed` en batería Compose/project.py específica. Posteriormente hubo correctivos de etiquetas de provider y Resource Preparation. Cada cifra pertenece a su propio checkpoint y no acredita por implicación el artifact 001C de 67 wheels ni backend artifacts de procesos.

## `.env`, Python y seguridad

`*.env.detail` explica parámetros **sin secretos**. Mappings DEV/UAT/PRD y resolución productiva de credenciales requieren qualification propia. Baseline Project Python `3.14.7`, imagen objetivo `python:3.14.7-slim-trixie`. Metadata ADA Generic y tooling Web implementados/ejecutados aquí aún usan **3.14.2**. Es una discrepancia transversal **OPEN / SEPARATE**: no tocarla silenciosamente en Master.

## Gates actuales

| Frontera | Estado |
|---|---|
| SOURCE_SMOKE/PORTABLE históricos | CLOSED para sus checkpoints, no transferible a HEAD actual |
| Resource Preparation 001 Docker local | CLOSED en alcance local de recursos; Home cold start OPEN |
| Material/generador Master 001A | CURRENT / IMPLEMENTED |
| Planner Master 001B | CURRENT / READ-ONLY |
| Master 001C página y login en distribución local | CURRENT / IMPLEMENTED / SMOKE USER-REPORTED PASS |
| Master 001C full qualification sobre HEAD final | OPEN / UNVERIFIED |
| Master `projection.apply` | PLANNED / SOLO DEBATE 001D |
| Master `users.replace` | BLOCKED / GATES PRODUCTIVOS Y DESTINO VACÍO |
| Warmup/upload productivo y baseline Entra excepcional | OPEN / CONTRACT + INTEGRATION |
| Azure real, telemetría externa, CI global | UNVERIFIED |

No ampliar Resource Preparation para absorber Master ni mezclar el siguiente debate 001D con Users, Home, migración Python o la construcción de artifacts backend.
