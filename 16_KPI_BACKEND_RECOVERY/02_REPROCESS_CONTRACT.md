# KPI Backend Recovery — Common Reprocess Contract

Estado: **CURRENT / IMPLEMENTED**

## Jobs autorizados

```text
KPI Runtime
KPI Historian
```

Default:

```text
REPROCESS_CURRENT=false
```

## Runtime

Con CURRENT forced:

```text
observed == committed
→ reevaluate same watermark
→ preserve durable evaluated_at_utc
→ same result => UNCHANGED
→ changed result => conflict
```

## Historian

Con CURRENT forced:

```text
authority == committed
→ after=None
→ replay durable batches through committed
→ rematerialize history/errors
→ commit same authority
```

## Nunca bypass

```text
regression checks
missing upstream authority/data
lease
cancellation
fencing
write conflict detection
```

Delivery y Timeseries no soportan este flag en la secuencia CURRENT.
