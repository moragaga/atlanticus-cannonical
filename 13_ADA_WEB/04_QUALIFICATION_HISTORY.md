# ADA Web — Qualification History

Estado: **CURRENT HISTORY / WEB STARTER DELTA 2026-09-26**

## Checkpoints históricos conservados

Los GREEN previos de Session/PWA/Wake/Activity/Card/Header, KPI Registry y KPI Definition no se reabrieron. El Collector, bootstrap operacional, wiring Collector y structural Operational Render Stage 1 conservan sus cierres en sus hitos originales:

- `atlanticus@01a4387d9f73aceb83441d2f26f94ad9025a661c`: ADA Generic bootstrap; `83 passed`, Ruff check/format PASS reportados.
- `atlanticus@940336d5b704d10280cc2375e68c60b45f235eb0`: hygiene de dependencias `pydantic` / `pydantic-settings`.
- `atlanticus@d6e405e6466b1bf8d29dadae442a03062da2f1b3`: Collector runtime wiring; `87 passed`, Ruff PASS reportados.
- `atlanticus@bc8eafc21a65e3f9aff044c232e2562cd490c49f`: Operational Render structural cutover; `7 + 56 + 86 = 149 passed`, Ruff y diff check reportados.

No atribuir estos resultados históricos al HEAD del cierre Web Starter.

## Web Starter — ejecución manual proporcionada por el usuario

Implementación inspeccionada: `atlanticus@c2bf25e353b890dc8fd8553ad375745d23ec7154`.

- Incrementos del tooling: ejecuciones separadas de `11 passed`, `8 passed`, `16 passed`, `18 passed`.
- Generic y ADA: `SOURCE_SMOKE / PASS`, CPython `3.14.2`.
- Wheelhouses: Generic `36` wheels; ADA `108`. Ambos: `PORTABLE / PASS` instalando offline desde copias fuera del checkout. Nueve checks cada uno: health.live, health.ready.diagnostic, home.http, example.http, dash.layout, module.page.registration, module.callback.registration, module.callback.http y module.assets.http.
- Incremento Docker 008: template `5 passed`, publicaciones ADA `2 passed`; las dos imágenes construyeron y contestaron `/health/live`. Generic sirvió `/example`; ADA devolvió HTML `Acceso denegado` para `/example` con Manager deshabilitado.

## Límite preciso

`PORTABLE / PASS` prueba instalación offline, no identidad productiva, navegación HTML autorizada ni Manager visual. Health Docker confirma liveness, no readiness funcional del Manager. Los Docker builds se generaron con wheelhouses cuyo `source_git_head` informado fue `7fcf1acc1ade0f3f07a42476f7c64dc0d2a85e64`; no afirmar que se reconstruyeron desde `c2bf25e...`. La probe no forzó `Accept: text/html` y no probó el mismo recorrido de browser en ADA.

**OPEN:** Manager Home/header/sidebar visible desde Starter, Navigation configurada que permita `/example`, UI visual Atlanticus, Gunicorn/8000 productivo, Entra real, Cosmos/Azurite/recovery, CI remoto y despliegue Azure. Mantener el Collector ya cerrado sin clonarlo.
