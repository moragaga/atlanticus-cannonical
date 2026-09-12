# KPI Backend Reprocessing — Safety Rules

Estado: **CURRENT DIRECTION**

`REPROCESS_CURRENT` NO significa:

- ignore errors;
- ignore authority;
- move watermark backwards;
- overwrite durable conflicts;
- disable lease;
- disable fencing;
- disable cancellation;
- publish without upstream data;
- fabricate a source watermark.

## Sí significa

```text
"sé que este watermark ya fue procesado;
quiero ejecutar otra vez la materialización asociada
usando la misma authority upstream."
```

## Production

Default siempre false.

Cuando true debe quedar visible en observabilidad:

```text
reprocess_current = true
```

y cada iteración forzada debe registrar que no fue una ejecución normal.

## Continuous jobs

Si el flag permanece true en un proceso continuo:

```text
cada poll
→ reprocesa current
```

Eso puede ser intencional para pruebas, pero es costoso.

Recomendación operacional:

```text
REPROCESS_CURRENT=true
+
run once / ejecución controlada
```

cuando el objetivo sea reparación puntual.

No introducir lógica auto-reset del flag dentro del proceso; configuration sigue siendo externa.
