# Alarm Engine — Source Ledger

Estado: **AUDIT LEDGER / UPDATED 2026-09-26**

## Fuentes HEAD inspeccionadas

```text
Implementation: moragaga/atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b
Canonical baseline: moragaga/atlanticus-cannonical@83cd871c8418e37d2c29dff30e2ea5ef54bda4a0
Historical decisions: moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Confirmado por consulta a Git: `411aea...` es un commit por delante de `3413b5ff3bf56ba1c3c158fe874ad573602dedcc`; ese commit incluye los incrementos Domain/B.2 y Web del routing. No inferir que el estado dirty que existió durante la validación local del usuario sigue existiendo en el commit actual.

## Genealogía de implementación relevante

```text
Pure B.2 resolver                  9398786ae9af7c00de1bcca9d7a311fe9ef2155f
Tools domain                       9b9600ae96c9153cf70d0fb401905963b8583c2f
Alarm Source v3/Tool manifest      d2a5e14822d3711e64668b8e70cfa15d7ddae2f0
Routing baseline anterior          3413b5ff3bf56ba1c3c158fe874ad573602dedcc
Routing completo en main           411aea44ac60c09d2b07ce41d34c3f378788b97b
```

Los checkpoints antiguos se conservan como genealogía, **no** describen automáticamente el HEAD actual.

## Evidencia aportada por el usuario en este hito

```text
Alarm domain:
uv run --python 3.14.2 --group dev pytest -q        GREEN (56 puntos reportados)
ruff check .                                         GREEN
ruff format --check .                               GREEN

Alarm B.2 Materialization:
uv run --python 3.14.2 --group dev pytest -q        GREEN (49 puntos reportados)
ruff check .                                         GREEN
ruff format --check .                               GREEN

Web Alarm Configuration, después de routing frontend:
uv run --python 3.14.2 --group dev pytest -q        GREEN (114 puntos reportados)
ruff check .                                         GREEN
ruff format --check .                               GREEN
```

Los conteos anteriores se obtienen de la salida compartida, no equivalen a un reporte CI ni a un rerun completo posterior al commit `411aea...`.

Preflight de los generadores de backend y frontend, `git apply --check` y `git apply` fueron reportados PASS. Las suites del host Configuration Manager y las pruebas de navegador posteriores al routing **no** se observaron. Tampoco se verificó E2E contra Blob/Cosmos real.

## Paths CURRENT relevantes

```text
scopes/ada-command-center/domain/alarms/src/.../routing_policy.py
scopes/ada-command-center/backend/alarms/materialization/src/.../resolver.py
scopes/ada-command-center/backend/alarms/materialization/tests/test_routing_direction.py
scopes/ada-command-center/web/alarms/configuration/src/.../web/authoring.py
scopes/ada-command-center/web/alarms/configuration/src/.../web/layout.py
scopes/ada-command-center/web/alarms/configuration/tests/test_alarm_routing_frontend.py
scopes/ada-command-center/web/alarms/configuration/src/.../source_projection.py
scopes/ada-command-center/web/alarms/configuration/src/.../projection_record.py
scopes/ada-command-center/web/alarms/projection-local/src/.../store.py
scopes/ada-command-center/web/alarms/projection-cosmos/src/.../store.py
scopes/ada-command-center/web/alarms/persistence/src/.../composition.py
scopes/ada-command-center/web/application/ada-command-center-configuration-manager/src/.../local_runtime.py
```

## Límites de esta auditoría

No hay manifest de ejecución del job B.2 porque todavía no existe dicho process en el árbol de código consultado. Que el paquete puro B.2 tenga un wheel en wheelhouse no demuestra que exista el proceso operacional.

No se escribieron archivos ni decisiones en Git durante este cierre. Los documentos en el ZIP son **propuestas de reemplazo local** para integrar tras revisión.
