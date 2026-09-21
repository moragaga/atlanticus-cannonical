# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Autoridad

```text
moragaga/atlanticus@d484569cbe0290f38f239481cde81b13a23deecf
```

## KPI backend — VERIFIED / preserved

Los cierres previos permanecen CURRENT:

```text
KPI Runtime recovery
Latest Delivery Registry consumption
Timeseries Delivery Registry consumption
Historian recovery
```

No fueron modificados por este hito.

## ADA KPI Collector — VERIFIED

Qualification observada en workspace real:

```text
scopes/ada/web/kpis/collector
ruff check                         PASS
ruff format --check                PASS
pytest                             PASS
scripts/scopes/ada/check.sh kpi-collector
                                   53 passed
commented mirrors                  PASS
```

Comportamientos cubiertos incluyen:

```text
Latest 10 s / Timeseries 120 s defaults
independent scheduling + Latest priority
source failure isolation
recovery after failure
incident deduplication
runtime critical observability
health/assets do not start poller
real request starts background poller
browser cache-only update
one store per ToolComponent
multi-worker browser monotonic merge
attachment duplicate rejection
real create_web_application smoke
```

## Web Observability — VERIFIED

Workspace real:

```text
web/framework/observability
ruff check          PASS
ruff format --check PASS
pytest              PASS
```

## Atlanticus Web Core — VERIFIED

Workspace real:

```text
web/framework/core
ruff check          PASS
ruff format --check PASS
pytest              PASS
```

La suite verifica que la `WebObservability` owned por el framework queda disponible en el
`ServiceRegistry` congelado mediante `WEB_OBSERVABILITY_SERVICE_KEY`.

El assert histórico `len(runtime.services) == 0` fue removido por ser un test stale de
implementación interna: una aplicación mínima sigue sin capabilities opcionales, pero sí posee
infraestructura base Web.

## ADA Generic Application — VERIFIED / preserved

Gate oficial observado después del incremento:

```text
scripts/scopes/ada/check.sh application
63 passed
commented mirrors PASS
```

Esto demuestra que hacer integrable el collector no convirtió a Generic Application en un
consumidor obligatorio.

## Repository hygiene — VERIFIED

```text
git diff --check
PASS
```

## UNVERIFIED / SEPARATE

```text
remote CI
full monorepo pytest
full workspace Ruff
python:3.14.7-slim-trixie global qualification
Azure productive wiring of collector resources
actual operational Tool + Cosmos collector mounting
```

La última línea es el siguiente foco; no confundir capability qualification con deployment/wiring
operacional ya realizado.
