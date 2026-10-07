# Atlanticus — Validation Baseline

Estado: **CURRENT — WEB GLOBAL INDICATOR / PRESENTATION-STORE HITO QUALIFIED LOCALLY 2026-10-06**

## Autoridad

```text
Implementation  moragaga/atlanticus@d97118d202dc1ea5ef3b0d1c18355d3a330824aa
Canonical base  moragaga/atlanticus-cannonical@af936617ebf4e04157e4ef905dd7bda598c1d05a
```

## KPI Collector presentation-store qualification

Reportado por el usuario:

```text
ada-web-kpi-collector
56 passed
ruff check .
PASS
ruff format --check .
PASS
```

Acredita el incremento donde Tool `READY` puede materializar stores de presentación aun sin KPI Delivery.

## ADA Generic focused qualification

Reportado por el usuario durante store materialization:

```text
tests/test_bootstrap.py
tests/test_external_composition.py
tests/test_integrated_manager_runtime.py
tests/test_operational_collector_integration.py

17 passed
```

Después del refactor genérico final:

```text
tests/test_application.py
37 passed
```

## Integrated Operations Global Indicator qualification

Reportado por el usuario:

```text
test_global_indicator_catalog.py
test_global_indicators.py

13 passed
```

Focused runtime Ruff:

```text
PASS
```

## Generic Global Indicator qualification

Reportado por el usuario:

```text
ada-web-ui-global-indicator
30 passed
ruff check .
PASS
```

Package-wide format check:

```text
Would reformat: tests/test_presentation.py
```

Ese archivo no fue tocado por el incremento y se conserva como drift preexistente/no relacionado.

## Lock qualification

Reportado por el usuario:

```text
uv lock --check
PASS
```

para:

```text
ada-web-ui-global-indicator
ada-generic-application
ada-integrated-operations-application
```

## Visual evidence before final generic refactor

Reportado por el usuario:

```text
Global Indicators visible without KPI Delivery
CONSTRUCTION overlay works in NORMAL
AUTHORING exposes the real GI composition
mobile shows all GI after height correction
desktop compaction/height acceptable
MINE / PLANT switching works
KPI detail/inspection works
```

## Post-refactor visual status

El último cambio movió sizing/responsive desde IO al paquete genérico y preservó las clases IO de scope.

Automated qualification quedó GREEN.

No se reportó una captura/smoke visual posterior a ese último traslado.

Por tanto:

```text
generic refactor automated qualification   VERIFIED
final mobile/desktop visual smoke           UNVERIFIED
final MINE/PLANT browser smoke              UNVERIFIED
final KPI Inspection browser smoke          UNVERIFIED
```

Estos puntos no invalidan los tests, pero deben mantenerse explícitos.

## Existing process-deployment qualification

El baseline canónico previo de process deployment/resource ownership permanece sin cambio.

No fue reabierto por este hito.

## No acredita

```text
post-refactor browser visual smoke
alarm-management integration
alarm-status integration
first alarms-flow card
Alarm Engine migration
Alarm Runtime data-input migration
History/Analytics
Python 3.14.7/Trixie migration
production Azure / Entra qualification
```

## Qualification scope

Esta qualification es local.

No equivale a CI, Azure ni productivo.
