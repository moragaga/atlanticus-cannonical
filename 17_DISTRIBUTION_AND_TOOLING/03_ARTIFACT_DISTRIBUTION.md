# Artifact and Distribution Boundary

Estado: **CURRENT — ADA WEB STARTER THINNING CLOSED / DISTRIBUTION PRECHECK CLOSED / RUNTIME E2E OPEN**.  
Corte implementado y trazable: `atlanticus@a75465745e188da4765e803595b17acaa55d9306` (2026-10-01).  
Decisions inspeccionado: `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.  
Canonical base inspeccionado antes de este reemplazo: `atlanticus-cannonical@e5f22298b9a8d62182cf9dc5bcad46c971faf261`.

Ningún resultado histórico recalifica automáticamente este corte. El artifact final de este hito declara `source_git_head=a75465745e188da4765e803595b17acaa55d9306`, coincidente con `atlanticus:main` remoto verificado al cierre.

## Frontera y ownership CURRENT

```text
SOURCE
  ↓
GENERATION / DISTRIBUTION TOOLING
  ↓
GENERATED ADA TOOL PROJECT
  ↓
DISTRIBUTION ARTIFACT
  ↓
HOST / DEVOPS / RUNTIME
```

Atlanticus produce artifacts, contratos de generación/distribución y runtime reutilizable. El host/DevOps es dueño de infraestructura, pipeline, secretos productivos y operación productiva.

La aplicación ADA generada no debe contener una segunda implementación completa del runtime o del tooling reusable por conveniencia. El código reusable pertenece a paquetes Atlanticus/ADA o al tooling compartido; la Tool generada conserva únicamente sus superficies de extensión, host/configuración/deployment y los entrypoints del proyecto que correspondan.

`distribution/` continúa siendo salida generada.

## ADA Web Starter CURRENT

`tooling/distribution/web/generate_starter.py` conserva perfiles `generic` y `ada`.

Para perfil ADA, el generador ya no hereda del starter base:

```text
docker/
src/application/modules/
src/application/pages/
```

En consecuencia, ADA no copia el demo Generic `modules/example`, no copia el Home Generic y no copia el probe Docker Generic. El overlay ADA entrega sus propias superficies de aplicación.

La Tool ADA generada contiene como contrato explícito de extensión:

```text
src/application/pages/home.py
src/application/pages/__init__.py
src/application/modules/__init__.py
```

`src/application/composition.py` parte de `create_local_operational_composition()` y:

- conserva los módulos y layout de ADA Generic;
- agrega `create_application_modules()`;
- reemplaza `page_packages` por `('application.pages',)`.

El Home generado registra `/` y es código editable de la Tool concreta. No es un demo Generic ni un reemplazo del shell ADA.

## Ownership ADA Generic vs Tool generada

ADA Generic continúa siendo dueño de las capacidades reutilizables de aplicación, incluyendo la composición operacional, shell/header, Navigation, branding, runtime experience y la integración de Manager cuando existen sus dependencias/stores.

La Tool generada es dueña de su contenido específico:

```text
Home
pages adicionales
módulos/callbacks propios
assets y presentación específica
integraciones particulares del host cuando correspondan
```

No se introdujo contrato legacy ni adaptador temporal para conservar el demo anterior.

## Project tooling CURRENT

La implementación pesada que antes vivía copiada en:

```text
tooling/distribution/web/starter/ada/tooling/project.py
```

fue extraída a un paquete reusable de distribución:

```text
tooling/distribution/web/ada/project-tooling/
  pyproject.toml
  src/ada_project_tooling/cli.py
```

Paquete:

```text
ada-project-tooling==0.1.0
```

El `project.py` distribuido es ahora un bootstrap delgado. En checkout de desarrollo puede cargar la implementación reusable desde el repositorio; en una distribución carga `ada_project_tooling/cli.py` desde el wheel interno y valida previamente identidad y SHA256 contra `wheelhouse/manifest.json`.

Si el wheel falta, es ambiguo, tiene identidad inválida, no coincide con el SHA256 o no puede leerse, el bootstrap falla cerrado.

Esta extracción reemplaza como CURRENT la descripción anterior del `project.py` pesado copiado íntegramente a cada Tool. El comportamiento histórico de 001D.4 se conserva como antecedente, pero su ownership ya no describe el estado actual.

## Human command surface CURRENT

Para comandos de tooling Web destinados a ejecución humana, la interfaz soportada es multiplataforma:

```text
<command>.py   implementación
<command>.sh   launcher Linux/macOS
<command>.cmd  launcher Windows
```

Actualmente este contrato está aplicado a:

```text
tooling/distribution/web/generate_starter
tooling/distribution/web/build_wheelhouse
tooling/distribution/web/qualify_starter
tooling/distribution/web/ada/build_distribution
tooling/distribution/web/ada/qualify_distribution
```

La Tool ADA generada conserva además:

```text
tooling/project.py
tooling/project.sh
tooling/project.cmd

tooling/master_projection.py
tooling/master_projection.sh
tooling/master_projection.cmd
```

Los `.sh` y `.cmd` son launchers mínimos; la lógica no se duplica en ellos.

Scripts Python internos que no son interfaz humana, como probes/verificadores o módulos runtime, no requieren launcher por este contrato.

## Build de distribución CURRENT

`tooling/distribution/web/ada/build_distribution.py`:

- construye los wheels internos declarados por el runtime ADA;
- construye adicionalmente `ada-project-tooling`;
- exige wheels internos portables `py3-none-any`;
- mantiene requirements externos/host/build con hashes;
- genera `wheelhouse/manifest.json`;
- registra `source_git_head`;
- genera `requirements/project.lock.json`.

El artifact trazable reportado para `atlanticus@a75465745e188da4765e803595b17acaa55d9306` produjo:

```text
status: BUILT_UNQUALIFIED
profile: ada
delivery_strategy: internal-wheels-external-image-build
internal_wheels: 69
source_git_head: a75465745e188da4765e803595b17acaa55d9306
```

`qualify_distribution` reportó:

```text
status: PRECHECK_PASS
internal_wheels: 69
image_build: UNVERIFIED
runtime: UNVERIFIED
```

`PRECHECK_PASS` valida la frontera implementada por ese precheck; no equivale a qualification Docker, runtime Web ni producción.

## Qualification de este hito

VERIFIED por ejecución local reportada por el usuario sobre el incremento integrado:

```text
33 tests seleccionados: PASS
Ruff del scope del incremento: PASS
sh -n de los launchers Web/ADA: PASS
generate_starter.sh: PASS
build_distribution.sh: BUILT_UNQUALIFIED
qualify_distribution.sh: PRECHECK_PASS
project.sh --help sobre artifact final trazable: PASS
source_git_head == atlanticus:main: PASS
```

Los `.cmd` existen y están cubiertos por el contrato/test estático, pero no fueron ejecutados físicamente en Windows en este hito.

Un smoke anterior al commit final observó `project.sh init --copy-env = SYNCED` y `master_projection.sh --help`, pero ese resultado no se usa para recalificar el artifact final por herencia. Debe repetirse cuando la qualification runtime lo requiera.

## Historial preservado — Resource Preparation y Master

Los resultados históricos de Resource Preparation 001 y Master 001A–001D siguen siendo evidencia de sus respectivos cortes y no se eliminan por este cambio.

La distribución histórica de 67 wheels, los cortes `9c6daffd...` y `ca3ee508...`, y sus resultados Docker/Manager/Master permanecen como evidencia histórica. No describen la distribución CURRENT de 69 wheels ni califican automáticamente `a7546574`.

El Starter ADA CURRENT todavía contiene responsabilidades de runtime/host y Master, entre ellas:

```text
src/application/local_resources.py
src/application/master_projection/material.py
src/application/master_projection/provision.py
src/application/master_projection/reader.py
src/application/runtime.py
src/application/production.py
src/application/wsgi.py
```

Este hito no decidió moverlas ni eliminarlas. Su ownership adicional queda fuera de este cierre; no removerlas por inferencia.

## Gates actuales

| Frontera | Estado real |
|---|---|
| ADA starter thin: exclusión de demo/pages/docker Generic | CLOSED / VERIFIED |
| Home/pages/modules específicos de la Tool | CLOSED / VERIFIED |
| Extracción de project tooling reusable | CLOSED / VERIFIED |
| Launchers `.sh` para comandos humanos Web/ADA | CLOSED / VERIFIED |
| Contrato `.cmd` presente | CURRENT / UNVERIFIED físicamente en Windows |
| Build distribución `a7546574` | CLOSED para build: `BUILT_UNQUALIFIED`, 69 wheels |
| Precheck distribución `a7546574` | CLOSED para precheck: `PRECHECK_PASS` |
| Trazabilidad `source_git_head` | CLOSED / VERIFIED |
| `project.sh --help` artifact final | CLOSED / VERIFIED |
| `project init/sync` sobre artifact final trazable | UNVERIFIED |
| Docker image de `a7546574` | UNVERIFIED |
| Runtime Web de `a7546574` | UNVERIFIED |
| Home/header/Navigation/Manager E2E | UNVERIFIED |
| Cosmos + Azurite sobre artifact final | UNVERIFIED |
| Windows `.cmd` execution | UNVERIFIED |
| Azure/Entra productivo | UNVERIFIED |

## Python, imagen y secretos

Baseline objetivo del Project:

```text
Python 3.14.7
python:3.14.7-slim-trixie
```

ADA Generic/Starter/distribución CURRENT continúan fijados en Python `3.14.2`; la imagen histórica/current asociada sigue fuera del baseline objetivo. La migración 3.14.2 → 3.14.7 y slim-bookworm → slim-trixie permanece **OPEN / SEPARATE** y no debe realizarse por efecto lateral.

`.env.detail` sigue siendo contrato documental/configurable y no debe exponer secretos. Este hito no completó la auditoría global de `.env.detail` ni decidió todos los valores que puede asignar el sistema.

## Estado y siguiente frontera

CLOSED en este hito:

```text
ADA starter thinning
Tool-specific Home/pages/modules
project tooling extraction
human .sh/.cmd command contract
distribution build precheck
source traceability
```

OPEN para la siguiente etapa:

```text
canonical reconciliation de este documento
.env.detail audit/configuration
Cosmos/Azurite local
project init/sync sobre artifact final
Web runtime
Home → header → Navigation → Manager E2E
Docker qualification
```

**NEXT TÉCNICO:** levantar y configurar el artifact trazable `a75465745e188da4765e803595b17acaa55d9306`, sin rediseñar nuevamente el generador. Primero auditar/completar `.env.detail`, luego preparar infraestructura local y finalmente verificar funcionalmente Home, shell/header, Navigation y Manager.
