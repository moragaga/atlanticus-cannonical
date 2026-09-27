# Distribution and Tooling — Source Ledger

Estado: **AUDIT LEDGER / WEB STARTER HISTORY PRESERVED + NEW AUDIT BOUNDARY 2026-09-27**

## Autoridades para este cierre

```text
Implementation inspected now: moragaga/atlanticus@392ee281a32396516fb08c23c63514d8cbdb3489
Canonical read before replacements: moragaga/atlanticus-cannonical@a4c813bf6c833455ebe4f5f0a5968b0633c7b045
Historical decisions: moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Web Starter historical checkpoint: moragaga/atlanticus@c2bf25e353b890dc8fd8553ad375745d23ec7154
Historical canonical inspected then: moragaga/atlanticus-cannonical@a87fce8f8ccb7384aa350f46b66e615cae68bbc7
```

Git permanece **SOLO LECTURA**. Las pruebas Docker, wheelhouse y smoke se conocen por salidas de terminal del usuario; la nueva consulta de HEAD es evidencia **estática**, no ejecución remota del asistente.

## Código inspeccionado en cierre Web Starter anterior

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

## Evidencia VERIFIED — reportada por el usuario en cierre histórico

1. Incrementos de tooling: ejecuciones específicas con **11**, **8**, **16** y **18** tests. No sumar como una suite única ni inferir monorepo GREEN.
2. `SOURCE_SMOKE/PASS` para `generic` y `ada` con CPython `3.14.2`; nueve checks cada uno: health.live, health.ready.diagnostic, home.http, example.http, dash.layout, module.page.registration, module.callback.registration, module.callback.http, module.assets.http.
3. Wheelhouses: Generic **36** y ADA **108** ruedas; ambos `PORTABLE/PASS` offline instalando copias fuera del checkout.
4. Docker 008: **5** tests template, **2** tests ADA publications root; generación de Starter+wheelhouse y construcción de dos imágenes locales. Ambos respondieron a `/health/live`; Generic sirvió `/example`; ADA devolvió HTML `Acceso denegado` con Manager deshabilitado.

Source SHA reportados: `a3514056c27a42ac89949cb2bc56008438591fb2` para primeros wheelhouses y `7fcf1acc1ade0f3f07a42476f7c64dc0d2a85e64` durante Docker 008. HEAD de cierre Web posterior `c2bf25e...` y HEAD actual `392ee281...` **no son los source HEAD acreditados de esos builds**.

## Finding histórico de Starter

El middleware autoriza documentos HTML sólo en rutas Navigation permitidas o por override legítimo. La probe histórica `client.get('/example')` no forzaba `Accept: text/html`: PASS no certificaba ruta browser. En Docker ADA se configuró `ADA_MANAGER_PERSISTENCE_PROVIDER=disabled`; no se verificó Manager Home/header/sidebar ni publicación/proyección real de Navigation desde Starter. Eso no significa que se quitara Manager del core.

## Historial preservado

Auditoría anterior `atlanticus@a6061ffed59c8b04e64b0a7fdc17050ef463c850` sobre bundler backend y scripts identificó referencia `scripts/local-process.sh` obsoleta; no tratarla como CURRENT sin verificar. No se reejecutó el bundler backend en el cierre Web Starter.

`manager_decisions/ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md` (**APROBADO / CONGELADO**) exige Manager genérico, `/manager`, cards/sidebar desde registry común con visibilidad autorizada y workflow separado. No autoriza bypass de Navigation ni fusionar headers.

## Delta de este cierre — scope local únicamente

- 01C: usuario aportó **16 + 12** tests y diff check PASS; 01D: el usuario aceptó visualmente header sin nombre, con marcas ADA/Atlanticus y `Usuarios`. No consta aquí salida de tests final 01D.
- 02 corregido: el usuario aceptó visualmente Jane rosa y John azul en **avatar e insignia Local**, con selección automática. El ZIP 02 inicial de insignia siempre azul quedó SUPERSEDED. No consta aquí la salida terminal de la suite final corregida.
- HEAD inspeccionado `392ee281...` contiene cambios de header, ADA branding, `title='Usuarios'`, `navigation_binding.py` con `LOCAL_USERS` y `tests/test_local_navigation_avatar.py`; inspección estática no equivale a test ejecutado ni wheelhouse regenerado.

## Nuevo inventario estático de frontera distribución

- `tooling/distribution/web/starter/base/Dockerfile` aún indica `python:3.14.2-slim-bookworm`, puerto `8050`, `python -m application` y modo local-only.
- `tooling/distribution/web/build_wheelhouse.py` aún exige CPython `3.14.2`.
- `deployment/local/generate_compose.py` genera workspaces **de artifacts de procesos**, no se ha acreditado que posea los modos propuestos `infra/app/full` del stack integrado Web+Cosmos+Azurite.
- `scopes/ada/web/application/ada-generic-application/.env.detail` contiene referencia a providers y conexiones, pero este hito no auditó la completitud de cada variable de cada proyecto ni ningún mapping de pipeline.

## UNVERIFIED / SEPARATE

Visual del Manager desde Starter, navegación HTML permitida, host productivo Gunicorn/8000, Entra, Cosmos/Azurite real/emulado y recuperación durable, Azure deployment, CI remoto, full-workspace Ruff y tests monorepo, nuevo wheelhouse desde el HEAD auditado, topología Compose integrada `infra/app/full` y `.env.detail` completos. No marcar una dirección aprobada como implementación realizada.

**NEXT único:** `DISTRIBUTION-ARTIFACTS-CURRENT-AUDIT` en nuevo chat: localizar y ejecutar/inspeccionar scripts y contracts reales antes de proponer cambios. Docker, env/pipeline y Compose quedan PLANNED para incrementos posteriores.
