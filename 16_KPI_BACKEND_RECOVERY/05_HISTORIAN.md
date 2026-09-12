# KPI Backend Reprocessing — Historian

Estado: **PROPOSED**

## Gate actual

Historian salta cuando:

```text
historian authority == KPI committed
```

## Particularidad

No basta con ignorar `SKIPPED_CURRENT`.

El algoritmo normal usa:

```text
read_after(historian_before, through=committed)
```

Si historian_before == committed:

```text
read_after(committed, committed)
→ empty
```

## Semántica de repair

Con `REPROCESS_CURRENT=true`:

```text
ignore historian progress only for read range
after = None
through = KPI committed
```

Luego:

```text
read all durable evaluation batches
→ historian materializer
→ merge history/errors
→ commit authority = same KPI committed
```

## Por qué full rebuild

Si el materializado histórico fue borrado o quedó inconsistente, no sabemos qué subconjunto falta.

La authority KPI sí conoce el límite válido.

Los persisted evaluation batches son la fuente durable para reconstrucción.

Por tanto, la primera versión de repair favorece:

```text
correctness > optimization
```

y reconstruye todo hasta current.

Más adelante puede existir `reprocess_from`, pero no es necesario para el primer contrato.

## Idempotencia

El materializer actual usa merge con keys estables.

Reprocesar contenido existente debe converger sin duplicarlo.

## No cambia

- KPI committed missing → EMPTY/error según contrato actual;
- historian authority ahead of KPI committed → ERROR;
- lease/cancellation/fencing permanece.
