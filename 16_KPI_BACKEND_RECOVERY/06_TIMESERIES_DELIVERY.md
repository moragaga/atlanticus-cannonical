# KPI Backend Reprocessing — Timeseries Delivery

Estado: **PROPOSED**

## Gate actual

Timeseries Delivery salta cuando:

```text
checkpoint == aligned historian watermark
AND
configuration revision == current
```

## Con REPROCESS_CURRENT

Ignorar sólo ese gate.

Luego:

```text
read historian authority
→ read history window
→ project timeseries
→ publish
→ commit same checkpoint
```

## Uso

Permite reconstruir:

- snapshot eliminado;
- publicación de prueba;
- resultado después de rematerializar Historian.

## No cambia

- missing Historian authority sigue sin producir series válidas;
- authority regression sigue ERROR;
- aligned watermark sigue siendo el mismo;
- no se fabrica historia.
