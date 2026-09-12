# KPI Backend Reprocessing — Latest Delivery

Estado: **PROPOSED**

## Gate actual

Latest Delivery salta cuando:

```text
checkpoint.watermark == KPI committed
AND
checkpoint.configuration_revision == current config
```

## Con REPROCESS_CURRENT

Ignorar únicamente `SKIPPED_CURRENT`.

Luego:

```text
read committed batch
→ project latest
→ publish snapshot
→ commit same checkpoint
```

## Casos

### Snapshot fue eliminado

```text
checkpoint current
snapshot missing

REPROCESS_CURRENT
→ republish
```

### Snapshot todavía existe e idéntico

Publisher puede devolver:

```text
UNCHANGED
```

y el checkpoint sigue igual.

### Evaluation batch falta

Sigue siendo error:

```text
KPI evaluation batch is missing
```

No inventar datos de Delivery.

## Authority

Delivery nunca lidera al KPI committed watermark.

La validación de autoridad se mantiene.
