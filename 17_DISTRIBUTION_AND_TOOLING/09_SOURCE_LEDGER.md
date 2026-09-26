# Distribution and Tooling — Source Ledger

Estado: **AUDIT LEDGER / 2026-09-26 WEB STARTER CLOSURE**

## Autoridades para este cierre

```text
Implementation inspected: moragaga/atlanticus@c2bf25e353b890dc8fd8553ad375745d23ec7154
Canonical before candidate replacement: moragaga/atlanticus-cannonical@a87fce8f8ccb7384aa350f46b66e615cae68bbc7
Historical decisions: moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Las pruebas Docker, wheelhouse y smoke se conocen por salidas de terminal del usuario; este cierre fue una auditoría de fuentes y evidencia, no una ejecución remota del asistente.

## Código inspeccionado en implementación CURRENT

```text
tooling/distribution/web/{generate_starter,build_wheelhouse,qualify_starter,probe_starter}.py
tooling/distribution/web/starter/base/{Dockerfile,.dockerignore,pyproject.toml,.python-version}
tooling/distribution/web/starter/ada/pyproject.toml
tooling/distribution/web/starter/{base,ada}/src/application/{composition,runtime}.py
tooling/distribution/web/starter/base/docker/verify_wheelhouse.py
tooling/tests/distribution/web/{test_generate_starter,test_build_wheelhouse,test_qualify_starter,test_container_template}.py
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/{application,__main__,bootstrap,composition}.py
scopes/ada/web/application/ada-configuration-manager/src/ada/web/application/configuration_manager/composition.py
web/capabilities/manager/src/atlanticus/web/manager/web/layout.py
web/capabilities/navigation/core/src/atlanticus/web/navigation/authorization.py
```

## Evidencia VERIFIED — reportada por el usuario

1. Incrementos sucesivos del tooling: ejecuciones específicas de `11`, `8`, `16` y `18` pruebas aprobadas. No sumar como suite única ni inferir full monorepo GREEN.
2. `SOURCE_SMOKE / PASS` para `generic` y `ada` con CPython 3.14.2; nueve checks por perfil: health.live, health.ready.diagnostic, home.http, example.http, dash.layout, module.page.registration, module.callback.registration, module.callback.http, module.assets.http.
3. Wheelhouse `generic`: 36 wheels. Wheelhouse `ada`: 108 wheels. Los dos reportaron `PORTABLE / PASS` al instalarse offline desde copias `/tmp` fuera del checkout, con CPython 3.14.2.
4. Docker 008: `5 passed` para template y `2 passed` para ADA publications root. Generación del Starter y ambos wheelhouses; construcción exitosa de dos imágenes locales. Ambos contenedores respondieron `/health/live`; Generic entregó `/example`. ADA devolvió HTML `Acceso denegado` para `/example` al ejecutarse con Manager deshabilitado.

SHA de origen reportado para primeros wheelhouses: `a3514056c27a42ac89949cb2bc56008438591fb2`. Durante Docker 008 los wheelhouses reportaron `7fcf1acc1ade0f3f07a42476f7c64dc0d2a85e64`. El HEAD de cierre `c2bf25e353b890dc8fd8553ad375745d23ec7154` es posterior: **no afirmar que las imágenes fueron reconstruidas desde este último SHA**.

## Finding y calificación

El middleware actual sólo autoriza solicitudes documentales HTML a rutas Navigation permitidas (o por override administrativo legítimo). La probe previa `client.get('/example')` no fuerza `Accept: text/html`, de modo que el PASS no demuestra navegación HTML de navegador. En el contenedor ADA de prueba se estableció `ADA_MANAGER_PERSISTENCE_PROVIDER=disabled`: no existe evidencia de Manager integrado/visible en ese Starter, ni de publicación/proyección real de Navigation desde él. Esto no significa que el Manager haya sido eliminado del core.

## Historial preservado

Checkpoint de auditoría anterior: `moragaga/atlanticus@a6061ffed59c8b04e64b0a7fdc17050ef463c850` sobre bundler backend y scripts. Allí se identificó una referencia obsoleta `scripts/local-process.sh` que no debe tratarse como CURRENT sin verificación. No se reejecutó el bundler backend en este hito.

Decisión histórica inspeccionada: `manager_decisions/ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md` (**APROBADO / CONGELADO**). Exige Manager genérico, Home `/manager`, cards/sidebar desde `ManagerModuleRegistry`, visibilidad/autorización antes del render y separación del workflow administrativo. No autoriza bypass de navegación ni fusionar Manager con ADA.

## UNVERIFIED en este cierre

Visual del Manager desde Starter, navegación HTML permitida en ADA, host productivo Gunicorn/8000, Entra productivo, Cosmos/Azurite real/emulado y recuperación durable, Azure deployment, CI remoto, full workspace Ruff, tests full monorepo, wheelhouse reconstruido específicamente desde HEAD de cierre.
