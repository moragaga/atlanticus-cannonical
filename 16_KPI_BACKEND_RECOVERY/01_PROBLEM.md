# KPI Backend Reprocessing — Problem

Estado: **VERIFIED**

Los procesos actuales usan watermarks/checkpoints para evitar trabajo repetido.

Esto es correcto en producción.

Pero dificulta pruebas y reparación cuando se elimina un materializado o se corrige un error sin que llegue un watermark nuevo.

## KPI Runtime

Hoy:

```text
source observed == KPI committed
→ up_to_date
→ skip
```

Si el evaluation batch fue eliminado para reparar/reprobar:

```text
watermark sigue current
→ runtime no lo reconstruye
```

## Latest Delivery

Hoy:

```text
delivery checkpoint
==
KPI committed + config revision
→ SKIPPED_CURRENT
```

Si se borra el snapshot publicado:

```text
checkpoint sigue current
→ no republica
```

## Historian

Hoy:

```text
historian authority == KPI committed
→ SKIPPED_CURRENT
```

Si se borra/repara el dataset histórico:

```text
authority sigue current
→ no rematerializa
```

## Timeseries Delivery

Hoy:

```text
timeseries checkpoint == aligned historian watermark + config revision
→ SKIPPED_CURRENT
```

Si se elimina el snapshot:

```text
checkpoint sigue current
→ no republica
```

## Objetivo

Poder reconstruir materializaciones con datos upstream existentes sin:

- borrar manualmente todos los watermarks;
- retroceder authorities;
- esperar nueva data;
- modificar source timestamps;
- romper fencing/leases.

Esto es una capacidad operacional de repair/test, no un modo de logging.
