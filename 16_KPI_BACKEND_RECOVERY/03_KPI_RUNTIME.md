# KPI Backend Recovery — KPI Runtime

Estado: **CLOSED / VERIFIED / CURRENT**

Implementado:

```text
REPROCESS_CURRENT=false
→ observed == committed => up_to_date skip

REPROCESS_CURRENT=true
AND observed == committed
→ load same source watermark
→ evaluate
→ preserve evaluated_at_utc from durable current batch
→ commit same watermark
```

Invariantes:

```text
same durable content => UNCHANGED
changed result at same watermark => conflict
missing durable batch for committed watermark => explicit error
observed < committed => rejected
new watermark => normal flow
lease/cancellation/fencing preserved
```

Qualification observada:

```text
kpi-runtime 43 passed
kpis/persistence 10 passed
Ruff PASS
format PASS
git diff --check PASS
```
