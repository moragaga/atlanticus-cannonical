# KPI Backend Recovery — Historian

Estado: **CURRENT durable history / PLANNED rolling read model**

## CURRENT — durable authority

Historian mantiene su contrato durable actual:

```text
KPI evaluation batches
→ daily long history
→ error history
→ KpiHistorianAuthority
```

`REPROCESS_CURRENT` y full durable replay permanecen CURRENT y no fueron reabiertos en este hito.

La historia diaria long continúa siendo la autoridad durable.

## CURRENT implementación

En `atlanticus:main` Historian todavía no genera un rolling Timeseries read model.

```text
rolling current.parquet
= NOT YET IMPLEMENTED
```

## PLANNED — rolling Timeseries read model

Decisión de Project acordada para el siguiente incremento:

Historian debe producir además una proyección local regenerable para lectura rápida de Timeseries.

```text
durable long history
    = authority

rolling wide parquet
    = disposable/read-optimized projection
```

Historian no debe conocer:

```text
Tools
destination_keys
named output Cosmos connections
Timeseries Delivery checkpoints
```

## Invariantes acordados

```text
maximum physical horizon = 24 h
grid                      = 30 s
shape                     = wide
timestamp                 = UTC
write                     = atomic replacement
authority                 = durable history, not rolling file
```

El rolling contiene solo cobertura realmente observada.

No fabricar 24 horas de filas nulas.

Si la primera cobertura real comienza a `00:10:00`, el rolling comienza ahí.

Las ventanas lógicas completas y los `null` faltantes pertenecen a Timeseries Delivery.

## Incremental path

En operación normal Historian ya posee los batches nuevos.

No debe escribir durable history y volver a leerla para actualizar el rolling.

```text
new evaluation batches
    ├─> durable long history
    └─> rolling wide projection
```

El rolling:

```text
merge new 30-second grid points
drop timestamps older than watermark - 24 h
atomic replace
```

## Recovery path

Si el rolling desaparece o debe reconstruirse:

```text
if durable history exists
→ read up to last 24 h from durable history
→ filter exact 30-second grid
→ pivot wide
→ write rolling atomically

if durable history does not exist
→ start from currently processed batches
→ physical coverage grows naturally from 0 toward 24 h
```

No rellenar almacenamiento físico con horas pasadas artificiales en `null`.

## Missing semantics

Rolling:

```text
missing timestamp   = row may be absent
missing KPI column  = column may be absent until observed
missing/error point = no usable scalar value
```

Timeseries Delivery hidrata posteriormente una grilla lógica completa y representa ausencia como `null`.

## Coherencia con authority

PROPOSED / NEXT CONTRACT:

```text
durable history write
→ rolling update
→ HistorianAuthority commit
```

Timeseries no debe consumir una vista parcialmente avanzada.

El detalle exacto de metadata del rolling y su validación debe cerrarse antes de implementar.

## OPEN

```text
exact rolling filesystem path
Parquet schema/metadata exactos
handling de value_type changes within rolling horizon
exact atomic writer primitive
authority/rolling mismatch behavior contract
```
