# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Autoridad

```text
moragaga/atlanticus@3ca8c833df916a4e0812c76eaba84ee5fde8a1cc
```

## KPI Runtime recovery — VERIFIED

Observado en workspace real:

```text
processes/kpi-runtime/tests    43 passed
kpis/persistence/tests        10 passed
ruff check                    PASS
ruff format --check           PASS
git diff --check              PASS
```

Además se verificó comportamiento forced-current, preservación de `evaluated_at_utc`, conflicto durable ante resultado distinto y rechazo de regression.

## Delivery + Timeseries Registry cutover — VERIFIED

```text
processes/kpi-delivery/tests             32 passed
processes/kpi-timeseries-delivery/tests  22 passed
kpis/delivery/tests                      28 passed
ruff check                               PASS
ruff format --check                      PASS / 64 files formatted
git diff --check                         PASS
```

## Historian recovery — VERIFIED

```text
processes/kpi-historian/tests            37 passed
focused recovery tests                    4 passed
kpis/history/tests                       22 passed
kpis/persistence/tests                   10 passed
ruff check                               PASS
ruff format --check                      PASS / 23 files formatted
git diff --check                         PASS
```

El integration test verifica reconstrucción de history eliminado desde durable batch.

## UNVERIFIED / SEPARATE

```text
remote CI
full monorepo pytest
full workspace Ruff
python:3.14.7-slim-trixie global qualification
Azure productive wiring of the final KPI resources
```

Una ejecución global de backend pytest observó errores de collection por `tests.support` en múltiples paquetes.

Clasificación exacta:

```text
FULL BACKEND PYTEST
BLOCKED / TEST-COLLECTION TOPOLOGY
UNVERIFIED AS PREEXISTING
OUTSIDE KPI RECOVERY INCREMENTS
```

No convertirlo en PASS ni declararlo preexistente sin baseline comparativo.
