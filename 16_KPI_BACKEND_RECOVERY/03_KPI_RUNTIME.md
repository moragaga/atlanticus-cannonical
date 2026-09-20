# KPI Backend Recovery — KPI Runtime

Estado: **PLANNED / NEXT**

## CURRENT

`KpiRuntimeJob` hace:

```text
observed == committed
→ up_to_date
→ skip
```

Settings CURRENT no exponen `REPROCESS_CURRENT`.

## Cambio autorizado

Agregar:

```text
REPROCESS_CURRENT=false
```

y entregarlo al job por el wiring CURRENT.

Cuando:

```text
REPROCESS_CURRENT=true
AND observed == committed
```

omitir sólo el shortcut `up_to_date`.

Continuar:

```text
load(as_of=observed)
→ evaluate base KPI
→ evaluate over KPI
→ KpiEvaluationBatch
→ KpiPersistence.commit(same watermark)
```

## Casos obligatorios

Batch faltante:

```text
committed = T
source = T
batch T missing
→ rebuild T
```

Batch existente e idéntico:

```text
→ write_once unchanged
```

Batch incompatible:

```text
→ durable conflict
```

Regression:

```text
observed < committed
→ error aunque forced
```

Missing source watermark:

```text
→ no source fabrication
```

## No cambia

```text
catalog semantics
DataLoadPlan
source authority
lease
fencing
cancellation
KpiPersistence conflict semantics
```
