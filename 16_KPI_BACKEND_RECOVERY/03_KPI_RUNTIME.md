# KPI Backend Reprocessing — KPI Runtime

Estado: **PROPOSED**

## Gate actual

`KpiRuntimeJob` corta cuando:

```text
observed == committed
```

con:

```text
reason = up_to_date
```

## Con REPROCESS_CURRENT

Sólo para:

```text
observed == committed
```

se ignora ese shortcut.

El job continúa:

```text
load(as_of=observed)
→ evaluate KPI
→ build KpiEvaluationBatch
→ KpiPersistence.commit(same watermark)
```

## Persistencia actual

`KpiPersistence.commit()` permite el mismo watermark.

Sólo prohíbe:

```text
batch.watermark < committed
```

y `write_once` conserva integridad del batch.

Por tanto:

### Batch eliminado

```text
committed = T
source = T
batch T missing

REPROCESS_CURRENT
→ evaluate T
→ write batch T
→ committed permanece T
```

### Batch existente e idéntico

```text
→ write_once unchanged
→ committed permanece T
```

### Batch existente pero contenido distinto

```text
→ durable conflict
```

No reemplazar automáticamente.

Si se está probando una corrección de lógica para el mismo watermark:

1. eliminar explícitamente el batch defectuoso en el entorno de prueba;
2. conservar committed watermark;
3. activar `REPROCESS_CURRENT`;
4. reconstruir.

Esto reduce el borrado manual sin debilitar la autoridad durable.

## No cambia

- missing source watermark sigue EMPTY;
- source regression sigue ERROR;
- no KPI sigue EMPTY;
- lease/fencing permanece;
- no se avanza watermark ficticiamente.
