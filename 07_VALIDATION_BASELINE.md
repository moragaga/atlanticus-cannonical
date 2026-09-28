# Atlanticus — Validation Baseline

Estado: **CURRENT — baseline global preservada + cierre local Alarm Engine B2c.5c/B2c.5d, 2026-09-28**. Las cifras son evidencia local aportada por el usuario; no confundir con CI ni infraestructura real. Este reemplazo agrega el hito de Alarm Engine sin reabrir ni recalificar los frentes KPI/Web cerrados previamente.

## Autoridad y alcance

```text
KPI / ADA Web checkpoint histórico: moragaga/atlanticus@d484569cbe0290f38f239481cde81b13a23deecf
Alarm Engine HEAD confirmado:     moragaga/atlanticus@a799dc15105d3e037f36ab77129ef0cfa8999013
Decisions contrastadas:          moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

El HEAD actual de Alarm Engine no constituye rerun de los gates históricos de otros frentes. No mezclar sus recuentos con 443 de Alarm Engine.

## KPI backend — VERIFIED en hitos anteriores / preservado

```text
KPI Runtime recovery                                 CLOSED / CURRENT
Latest Delivery Registry consumption                CLOSED / CURRENT
Timeseries Delivery Registry consumption            CLOSED / CURRENT
Historian recovery                                  CLOSED / CURRENT
```

Estos frentes no se modificaron como parte de B2c.5d.

## ADA KPI Collector — gates locales históricos comunicados

```text
scopes/ada/web/kpis/collector
ruff check                              PASS
ruff format --check                     PASS
pytest                                  PASS
scripts/scopes/ada/check.sh kpi-collector    53 PASS
commented mirrors                       PASS
```

Entre los comportamientos cubiertos estaban defaults Latest 10 s/Timeseries 120 s, planificación independiente con prioridad Latest, aislamiento de fallos y recovery, incident deduplication, observability crítica, ausencia de poller al cargar health/assets, inicio por request real, actualización browser cache-only, un store por ToolComponent y merge monotónico multi-worker. El smoke de `create_web_application` fue verificado en ese checkpoint; no equivale a integración operacional con Cosmos real.

## Web Observability y Web Core — gates históricos

```text
web/framework/observability    ruff / format / pytest PASS
web/framework/core             ruff / format / pytest PASS
```

La suite Web Core confirma que `WebObservability` está disponible bajo `WEB_OBSERVABILITY_SERVICE_KEY` en `ServiceRegistry` congelado. El assert histórico `len(runtime.services)==0` quedó SUPERSEDED: una app mínima tiene infraestructura Web base aun sin capabilities opcionales.

## ADA Generic Application — gate histórico preservado

```text
scripts/scopes/ada/check.sh application     63 PASS
commented mirrors                         PASS
```

No convertir por esta evidencia al collector en consumidor obligatorio ni declarar integración operacional real terminada.

## Alarm Engine B2c.5c — gate local CLOSED

```text
scopes/ada-command-center/backend
pytest processes/alarms-runtime/tests integration_tests              119 PASS
pytest alarms/core/tests alarms/persistence/tests
       processes/alarms-materialization/tests
       processes/alarms-runtime/tests integration_tests               436 PASS
ruff check src/tests/integration scope informado                       PASS
ruff format --check del mismo scope                                   33 PASS
git diff --check                                                      PASS
```

Código presente en `atlanticus@672ed047f59459034d2fde23d05427e234bd01d5`. Demuestra fuente adapter, requisitos por consumidor y regresión controlada; no datasets físicos.

## Alarm Engine B2c.5d — gate local CLOSED después de traslado

```text
pytest test_example_threshold_catalog.py test_example_threshold_cycle.py   7 PASS
pytest alarms/core/tests alarms/persistence/tests
       processes/alarms-materialization/tests
       processes/alarms-runtime/tests integration_tests                    443 PASS
ruff check processes/alarms-runtime                                        PASS
ruff format --check src/tests                                              40 PASS
git diff --check                                                            PASS
```

La versión previa, con registro automático del ejemplo, tuvo 6 específicas y 442 de regresión PASS, pero un test requería formato; esa configuración quedó SUPERSEDED. El ejemplo finalmente se ubica en `catalog/examples/threshold` y el registro productivo queda vacío. HEAD remoto confirmado `atlanticus@a799dc15105d3e037f36ab77129ef0cfa8999013`. No declarar probado un nuevo wheel específico tras el traslado ni repetición de tests tras el commit en checkout limpio.

## Repository hygiene y límites

El usuario comunicó `git diff --check` sin hallazgos al verificar los incrementos; los archivos de actualización documental se generan fuera de Git para revisión. Python `==3.14.2` se usó en los gates Command Center porque su metadata actual lo exige; el objetivo de Project `3.14.7` no fue alcanzado por este hito.

**UNVERIFIED / SEPARATE:** remote CI, full monorepo pytest, full workspace Ruff, `python:3.14.7-slim-trixie` global, E2E Cosmos/Blob/Azure, datasets físicos operacionales, volumen multi-host, productores GREEN reales y composición operacional completa del nuevo catálogo/fuentes. La campaña física histórica F-010 del antiguo Alarm Engine tiene su propio contrato/evidencia en `04_ALARM_ENGINE/08_QUALIFICATION_BASELINE.md`, sin reinterpretarse como qualification B2c.
