# KPI Backend Recovery — Historian

Estado: **CLOSED / VERIFIED / CURRENT**

## Default

```text
REPROCESS_CURRENT=false

authority == KPI committed
→ SKIPPED_CURRENT
```

## Forced current

```text
REPROCESS_CURRENT=true
AND authority == committed
→ after=None
→ through=committed
→ replay all durable evaluation batches
→ materialize history/errors
→ commit same authority
```

Si authority < committed, aun con flag true se mantiene catch-up incremental.

Authority > committed continúa siendo error.

Qualification:

```text
historian suite     37 passed
focused recovery     4 passed
kpis/history        22 passed
kpis/persistence    10 passed
Ruff PASS
format PASS
git diff --check PASS
```

Integration test verificó rebuild de history eliminado desde durable batch.
