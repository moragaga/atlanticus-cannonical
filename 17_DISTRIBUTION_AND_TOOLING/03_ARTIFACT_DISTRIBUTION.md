# Artifact and Distribution Boundary

Estado: **CURRENT — ADA STARTER + MASTER 001A–001D.2 IMPLEMENTADOS; 001D.3 DOCKER NAVIGATION LOCAL CLOSED; 001D.4 SYNC CORRECTIVO CLOSED**.  
Corte de correctivo y distribución limpia: `atlanticus@9c6daffd04b9c249f75a55b6cdb9b44e6d92a795` (2026-09-28). Para la prueba funcional Docker de Master usar **su propio** corte `atlanticus@ca3ee5084542e393c105b49e98b3c282da56f7fb`. Canonical base: `0f2fff3ec0e71903b5703e03dd6050765d9722ff`. Ningún artifact se declara cualificado por herencia de otro SHA.

## Frontera y ownership

```text
SOURCE → ARTIFACT → DISTRIBUTION INPUT
```

Atlanticus produce artifacts y contrato de distribución; el host/DevOps es dueño de infraestructura, pipeline y secretos productivos. No confundir `deployment/local/generate_compose.py` de procesos backend con Starter Web, ni mezclar cualificaciones de wheelhouses históricos con las 67 ruedas de esta distribución ADA. `distribution/` es salida generada.

## Starter Web, Docker y qualification histórica conservada

`tooling/distribution/web/generate_starter.py` genera Starters editables Generic/ADA. Para ADA, `tooling/distribution/web/ada/build_distribution.py` genera wheelhouse/requirements/manifests; `qualify_distribution.py` realiza precheck de integridad y requisitos, **no** sustituye pruebas Docker/producción. Starter ADA usa Gunicorn, Dockerfile y Compose `infra.yaml`, `web.yaml`, `full.yaml`. `production.py` exige identidad productiva suministrada por host, no fabrica Entra.

Histórico independiente: SOURCE_SMOKE/PORTABLE Generic 36 y ADA 108 wheels con Python 3.14.2 y Docker 008 anteriores. Esos resultados **no** caracterizan la distribución actual de 67 wheels.

## Resource Preparation 001 — historial CURRENT/CLOSED local

Commit `da75752e87036b8318f38f8d405c55e8cb18717d`: job local espera Cosmos `/ready` y Azurite; prepara contenedor Blob, base Cosmos y seis contenedores físicos. `full.yaml` no hace depender la Web del éxito de `resources`; CLI vigente `ada-generic-manager-resources prepare|validate`, nunca `ensure-local`. En producción no crear automáticamente recursos Blob y la base Cosmos se gestiona externamente.

**VERIFIED USER-REPORTED de su checkpoint:** suite ADA Generic/tooling acotada, Ruff y diff check; distribución de 67 wheels, imagen local; ocho recursos creados/READY, reejecución idempotente, reinicio, diagnóstico parcial y recuperación de Cosmos. **OPEN histórico:** cold start con Cosmos ya detenido no produjo `/health/live` dentro de diez segundos; el mismo contenedor sí respondió tras recuperación, pero readiness integral/Home no quedó cualificada.

## Master 001C — hallazgos históricos preservados

El smoke artifact inicial de 001C (67 wheels, `BUILT_UNQUALIFIED`) respondió HTTP 500 en `/master-projection`: Navigation requería un `AccessSnapshot` Identity para una ruta que Master excluía. El middleware Master se registró antes de Identity/Navigation y la excepción se limitó a dos rutas exactas; el fallo anterior está **SUPERSEDED** por ese correctivo. El smoke v2 del período 001C observó `SYNCED`, HTTP 200 con/sin material y login informado, pero su manifest declaraba `source_git_head=efe231d61c9d5a6f4eca1e3f22a201a9b3c1861b` mientras el build había usado cambios locales previos al commit; no certificarlo como un artifact bit a bit del SHA anunciado.

El constructor requiere CPython 3.14.2 y `packaging`; en este hito se usó `uv run --no-project --python 3.14.2 --with packaging==25.0` tras observar un `ModuleNotFoundError` sin dicha librería. No se introdujo por ello una dependencia runtime adicional. Las pruebas 001C reportadas (38/38, 39/39, 3/3, 6/6, 2/2 en momentos diferentes) son **históricas, seleccionadas y solapadas**; no sumarlas ni atribuirlas a un único HEAD.

## Master 001A–001D.3 — material y ejecución

El Starter ADA contiene:

```text
tooling/distribution/web/starter/ada/tooling/master_projection.py
tooling/distribution/web/starter/ada/src/application/master_projection/{material,reader}.py
tooling/distribution/web/starter/ada/src/application/runtime.py
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/master_projection/{plan,composition,apply,web}.py
```

El generador usa material externo ZIP AES-256/scrypt y prompts interactivos; no admite contraseñas en argumentos. En runtime, `ADA_MASTER_PROJECTION_MATERIAL_PATH` es la **única** ruta opcional Master; ausencia/presencia/invalidez se controlan explícitamente. El ZIP declara preview/apply/users.replace, pero **solo preview y apply individual de seis proyecciones ordinarias** tienen controlador Master; Users REPLACE sigue no ejecutable.

**VERIFIED USER-REPORTED Docker 001D.3, distribución `ca3ee508`:** checkout/base manifest con ese SHA, **67 internal wheels**, `BUILT_UNQUALIFIED`, `PRECHECK_PASS`, `SYNCED`; material nuevo generado externamente con `service_user=master-service`, montado read-only en la Web Docker; login real en `/master-projection` y vista inicial de seis `SOURCE_MISSING`. Después de modificar/publicar Navigation, el operador ejecutó prepare/confirm desde Master y observó en Manager, tras recargar, estado **proyectado y sincronizado**. No se hizo prueba equivalente de los cinco dominios restantes, rollback, fallos parciales o operación Azure. El generador del checkpoint anterior requirió el workaround `PYTHONPATH=$PWD/src` en el host, retirado por 001D.4.

## 001D.4 — corrección de sincronización CURRENT/CLOSED

Implementación confirmada en `atlanticus@9c6daffd04b9c249f75a55b6cdb9b44e6d92a795`:

```text
tooling/distribution/web/starter/ada/tooling/project.py
tooling/distribution/web/starter/ada/commented/tooling/project.py
tooling/tests/distribution/web/ada/test_project_tool.py
```

Antes, `project.py sync` instalaba los wheels internos, pero **no** instalaba el paquete propio del Starter en `.venv`; ejecutar `application.master_projection.material` sin añadir `src` a `PYTHONPATH` podía fallar. El correctivo instala también un wheel del Starter usando el build requirements congelado que ya se distribuía, sin añadir parámetros ni nuevas variables de entorno. La huella de sincronización incluye digest del código Starter y revisión del esquema de instalación; si falla la construcción, no queda un stamp de sincronización válida. Se mantiene el contrato existente de `project.py run` en este incremento; no se reabre su modo editable ni se añaden wrappers legacy.

**VERIFIED USER-REPORTED TESTS:** Ruff PASS tras retirar cuatro imports sin uso, diff check PASS y **24/24 tests** de project tooling + Master tooling. El commit publicado `9c6daffd` contiene el correctivo separado del frente concurrente `kpi-runtime`.

**VERIFIED USER-REPORTED DISTRIBUCIÓN LIMPIA:** `git worktree` detached de `9c6daffd`, generado nuevo Starter, `BUILT_UNQUALIFIED` **67 wheels** y `source_git_head=9c6daffd`, `PRECHECK_PASS`, `project.py sync=SYNCED`, import real de `application.master_projection.material` **sin `PYTHONPATH`** (`STARTER_IMPORT_OK`), `tooling/master_projection.py --help` sin `PYTHONPATH`, segundo `sync=ALREADY_SYNCED`.

Este conjunto **cierra 001D.4 para sincronización local**. El help verifica invocación/import, **no** genera material nuevo ni autentica de nuevo sobre esa distribución.

## Gates actuales

| Frontera | Estado real |
|---|---|
| Resource Preparation 001 Docker local | CLOSED en su alcance; Home cold start sigue OPEN |
| Material 001A, planner 001B, HTTP 001C, apply 001D.1/001D.2 | CURRENT / IMPLEMENTED |
| 001D.3 Master→Navigation→Manager en Docker | CLOSED / VERIFIED USER-REPORTED LOCAL para Navigation |
| 001D.4 sincronización del Starter desde checkout limpio | CLOSED / VERIFIED USER-REPORTED LOCAL |
| Distribución `9c6daffd` | BUILT_UNQUALIFIED + PRECHECK_PASS + SYNCED, **image_build/runtime UNVERIFIED** para esa nueva distribución |
| Los otros cinco dominios Master en Docker | UNVERIFIED |
| Users REPLACE desde Master | BLOCKED; fuera de 001D |
| ZIP productivo, Key Vault/warmup/rotación, Entra/Azure | OPEN / UNVERIFIED |
| CI monorepo, múltiples workers y qualify end-to-end | UNVERIFIED |

## Python, secretos y foco siguiente

Baseline objetivo Project Python **3.14.7** e imagen **`python:3.14.7-slim-trixie`**. ADA Generic/Starter/distribución aquí verificados siguen fijados en **3.14.2**; **OPEN / SEPARATE**, no migrar por efecto lateral. `.env.detail` es documental, sin secretos; no añadir variables/rutas redundantes. El material Master real nunca se entrega dentro de artifacts ni en repositorios.

**NEXT TÉCNICO PROPUESTO:** después de la integración humana de este ajuste documental, calificar en Docker la distribución `9c6daffd`: arranque del Starter sincronizado como paquete, material Master externo, login y repetición del flujo Navigation → Master prepare/confirm → Manager recargado. No atribuirle automáticamente el resultado de `ca3ee508`. La validación de los otros cinco dominios, Users REPLACE, ADA Operational Identification y Azure siguen siendo incrementos separados.
