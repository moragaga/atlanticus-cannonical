# ADA Web — Qualification History

Estado: **CURRENT HISTORY / LOCAL MANAGER HEADER + COLORS CLOSED 2026-09-27**

## Checkpoints históricos conservados

- `atlanticus@01a4387d9f73aceb83441d2f26f94ad9025a661c`: ADA Generic bootstrap; 83 tests, Ruff check/format PASS reportados.
- `atlanticus@940336d5b704d10280cc2375e68c60b45f235eb0`: hygiene `pydantic` / `pydantic-settings`.
- `atlanticus@d6e405e6466b1bf8d29dadae442a03062da2f1b3`: wiring de Collector; 87 tests y Ruff PASS reportados.
- `atlanticus@bc8eafc21a65e3f9aff044c232e2562cd490c49f`: structural Operational Render cutover; 7 + 56 + 86 = 149 tests, Ruff y diff check reportados.

No atribuir estas suites históricas al HEAD del nuevo cierre.

## Web Starter — cierre manual 2026-09-26

Implementación entonces inspeccionada: `atlanticus@c2bf25e353b890dc8fd8553ad375745d23ec7154`. El usuario informó tests específicos del tooling de 11, 8, 16 y 18; SOURCE_SMOKE/PASS y PORTABLE/PASS Generic y ADA con CPython 3.14.2. Wheelhouses: Generic 36 y ADA 108 ruedas. Ambos instalaron fuera del checkout en modo offline y completaron nueve checks de probe. Las pruebas Docker 008 incluyeron template 5 tests y publications root ADA 2 tests; dos imágenes se construyeron y contestaron `/health/live`. Generic sirvió `/example`; ADA devolvió HTML `Acceso denegado` bajo Manager deshabilitado.

Límite: PORTABLE no prueba identidad productiva, autorización browser HTML, Manager ni persistencia real. Liveness Docker no equivale a readiness del Manager. Las imágenes no fueron reconstruidas desde el HEAD del nuevo cierre.

## Nuevo cierre: header Manager ADA local

- El usuario aportó para el correctivo 01C: `CHECK_PASS` y `APPLY_PASS`, 16 tests del Manager y 12 de ADA Configuration Manager aprobados y `git diff --check` sin hallazgos. No convertir 16+12 en un pase del monorepo.
- El incremento posterior 01D retiró el nombre del usuario del header. El usuario confirmó su **aceptación visual**: marcas ADA/Atlanticus, sin Los Pelambres, retorno/Manager Home y label `Usuarios`. No consta aquí salida del rerun final de tests 01D; queda **UNVERIFIED como ejecución**.
- En el último incremento 02 corregido, el usuario aceptó visualmente avatar **e insignia Local** Jane rosa `#C85D91` y John azul `#3778C2`, con selección automática. El ZIP original que dejaba la insignia siempre azul quedó **SUPERSEDED**. No consta aquí salida terminal de la suite final de 02 corregido.
- Inspección estática actual de `atlanticus@392ee281a32396516fb08c23c63514d8cbdb3489`: están las marcas finales, ausencia de principal en header, label ADA `Usuarios`, resolución local de ambos colores y tests `test_local_navigation_avatar.py`. Ello no prueba CI ni ejecución completa.

## Todavía OPEN / UNVERIFIED

Header/Manager desde **Starter ADA distribuido**, ruta `/example` con HTML autorizada por Navigation, Gunicorn/8000 local+productivo, Entra, Cosmos/Azurite/restart, pipelines, Azure, full-workspace Ruff, tests completos y rebuild del wheelhouse desde HEAD actual. Estos frentes no forman parte del cierre visual del core.
