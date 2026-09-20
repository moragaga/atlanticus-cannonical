# KPI Backend Recovery — Source Ledger

Estado: **AUDIT LEDGER / CLOSED**

## Final CURRENT cut

```text
moragaga/atlanticus@3ca8c833df916a4e0812c76eaba84ee5fde8a1cc
parent = 06e8f4ba4882c2d2600f995cce00b7bfdc01990c
tree   = b53495d710ae9307ce5b64da3311880d4bd6c050
```

## Runtime

Inspeccionado:

```text
processes/kpi-runtime/.../settings.py
processes/kpi-runtime/.../job.py
```

CURRENT:

```text
REPROCESS_CURRENT
preserved evaluated_at_utc on same-watermark replay
```

## Delivery

Inspeccionado:

```text
processes/kpi-delivery/.../configuration.py
storage.py
settings.py
composition.py
backend/kpis/delivery/models.py
backend/kpis/delivery/latest.py
```

CURRENT:

```text
Registry durable consumer
owned ada-kpi-latest-delivery output
container topology internal, database external
```

## Timeseries

Inspeccionado:

```text
processes/kpi-timeseries-delivery/.../configuration.py
storage.py
settings.py
composition.py
backend/kpis/delivery/models.py
backend/kpis/delivery/timeseries.py
```

CURRENT:

```text
Registry durable consumer
owned ada-kpi-timeseries-delivery output
schema v2
120-second series grid
```

## Historian

Inspeccionado:

```text
processes/kpi-historian/.../settings.py
job.py
composition.py
history.py
```

CURRENT:

```text
REPROCESS_CURRENT
forced-current full durable replay
incremental catch-up preserved
```

## Canonical conflict before replacement

`atlanticus-cannonical@961447d...` todavía describía estos cuatro frentes como PLANNED/BLOCKED y Delivery/Timeseries como legacy consumers.

Clasificación:

```text
IMPLEMENTATION CURRENT
CANONICAL STALE
REPLACEMENT REQUIRED
```
