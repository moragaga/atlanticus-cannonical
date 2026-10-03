# KPI Backend Recovery — Testing

Estado: **VERIFIED for Materialization + Latest + Historian rolling / TIMESERIES NEW DESIGN UNVERIFIED**

## Qualification verificada — Historian rolling

Resultado final del hito:

```text
kpis/history
27 passed
ruff check                 PASS
ruff format --check        PASS
15 files already formatted

processes/kpi-historian
47 passed
ruff check                 PASS
ruff format --check        PASS
24 files already formatted

git diff --check
PASS
```

El único finding mecánico durante qualification fue formato de
`processes/kpi-historian/tests/test_rolling.py`.

Se corrigió con Ruff y se repitió la suite completa del proceso:

```text
47 passed
All checks passed
24 files already formatted
```

## Qué acredita Historian

VERIFIED:

```text
shared rolling constants and metadata contract
metadata canonical JSON roundtrip
30 s alignment validation
historian_revision/watermark coherence
exact <application_root>/timeseries/current.parquet path
incremental update without durable-history reread
observed physical rows only
empty rolling coverage
wide Parquet shape
scalar value_type tracking
value_type transition resets prior logical series
JSON excludes/clears scalar rolling series
24 h physical trimming
durable-history rebuild
corrupt/missing rolling recovery
atomic replacement preserves previous committed file on failure
current coherent skip
current incoherent rebuild
durable history -> rolling -> HistorianAuthority ordering
rolling failure prevents authority advancement
composition and public API
```

## Qualification previa conservada

También permanece VERIFIED de hitos anteriores:

```text
kpi-materialization-runtime    14 passed
kpi-delivery-runtime           28 passed
kpi-connections                 7 passed
```

Esos resultados no se reabrieron en este hito.

## Política de tests

No crear tests cuyo único objetivo sea:

```text
assert a word does not exist
assert a class/function does not exist
freeze internal implementation shape
freeze visual CSS/markup structure
```

Probar:

```text
behavior
contracts
invariants
regressions
recovery
failure ordering
public integration surfaces
```

En Web, apariencia y responsive se validan visualmente salvo comportamiento funcional automatizable.

## UNVERIFIED / OPEN

```text
new Timeseries rolling consumer
new Timeseries logical hydration
new Timeseries multi-Tool parallel publication
new Timeseries per-Tool checkpoints
new Timeseries output grid/schema
real Cosmos multi-Tool integration
real Azure credentials
full Historian -> Timeseries operational E2E
RU/load/performance profile
```

No usar esos pendientes para reabrir Historian sin un finding concreto.
