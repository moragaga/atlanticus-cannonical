# Artifact and Distribution Boundary

Estado: **CURRENT IMPLEMENTATION PARTIAL / FULL DISTRIBUTION UNVERIFIED**

Inspeccionado: `moragaga/atlanticus@a6061ffed59c8b04e64b0a7fdc17050ef463c850`.

## Frontera congelada

```text
SOURCE → ARTIFACT → DISTRIBUTION INPUT
```

Atlanticus posee artifact y su contrato de entrega. El pipeline corporativo y su
implementación pertenecen a DevOps. No instalar un framework paralelo para reconstruir
lo que ya existe.

## Backend CURRENT observado

`deployment/processes/bundle.py` existe y descubre procesos exportables a partir de
`pyproject.toml` bajo los layouts `scopes/<scope>/processes/*` y
`scopes/<scope>/backend/processes/*`; usa `[tool.atlanticus.container]`, resuelve
dependencias internas, valida inputs y produce bundles de procesos. Se verificó
**existencia y contrato estático**, no se ejecutó build/distribution en este cierre.

Los procesos ADA KPI Runtime, Delivery, Historian y Timeseries Delivery tienen
`pyproject.toml` y archivos de configuración; no atribuirles éxito de un build final
sin ejecutar el bundler y sus tests sobre el checkout actual.

`scripts/scopes/ada/backend/check.py` posee gates y wheel build de capabilities backend.
El árbol inspeccionado **no contiene** `scripts/local-process.sh`, aunque un canonical
anterior lo describía como CURRENT. Esto se registra como discrepancia documental;
no inventar un sustituto con ese nombre.

## Web CURRENT observado

`scopes/ada/web/application/ada-generic-application/pyproject.toml` existe, declara
entrypoints `ada-generic-application` y `ada-generic-manager-resources`, y usa fuentes
locales del monorepo bajo `tool.uv.sources`. La Web ejecutó el flujo Navigation local.

La portabilidad de su build final, el cierre de wheels/dependencias, el entrypoint
fuera del monorepo, el setup de host productivo y un artifact Web integral permanecen
**UNVERIFIED**. Un `uv run` local no representa esa evidencia.

## Conflicto de runtime objetivo

Project baseline: `Python 3.14.7`, imagen objetivo `python:3.14.7-slim-trixie`.
Inspección del source CURRENT:

```text
deployment/processes/bundle.py                PYTHON_VERSION 3.14.2
deployment/processes/Dockerfile               python:3.14.2-slim-bookworm
ada-generic-application/pyproject.toml        requires-python ==3.14.2
scripts/scopes/ada/backend/check.py           EXPECTED_PYTHON_VERSION 3.14.2
```

**CONFLICT / PLANNED**: decidir y cualificar la alineación en el frente de productización.
No modificar silenciosamente versiones ni fingir compatibilidad.

## Separación de entregables

```text
KPI Collector existente                        CLOSED / CURRENT
Local Navigation configuration consumption     CLOSED / VERIFIED MANUAL
Backend process bundling infrastructure       IMPLEMENTED / UNQUALIFIED HERE
ADA Generic Web distributable                  PLANNED / UNVERIFIED
Real Tool Golden Path + target environment     OPEN
```

Primero auditar lo existente; después materializar únicamente la brecha demostrada.
