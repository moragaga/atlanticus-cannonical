# KPI Backend Recovery — Historian

Estado: **DECIDED / PLANNED**

## CURRENT

Historian salta cuando:

```text
historian authority == KPI committed
→ SKIPPED_CURRENT
```

El algoritmo normal usa:

```text
read_after(historian_before, through=committed)
```

Por eso simplemente omitir el skip no basta:

```text
read_after(committed, committed)
→ empty
```

## Cambio autorizado

Agregar:

```text
REPROCESS_CURRENT=false
```

Con forced-current:

```text
after = None
through = KPI committed
```

Luego:

```text
read all durable evaluation batches
→ materialize history/errors
→ commit authority = same KPI committed
```

## Razón

Si history fue borrado o quedó inconsistente, la primera versión de repair no conoce qué
subconjunto falta.

La authority KPI define el límite válido.

Los evaluation batches persistidos son la fuente de reconstrucción.

```text
correctness > optimization
```

## No cambia

```text
KPI committed missing behavior
historian authority ahead of KPI → error
lease
cancellation
fencing
```

`reprocess_from` queda fuera de alcance.
