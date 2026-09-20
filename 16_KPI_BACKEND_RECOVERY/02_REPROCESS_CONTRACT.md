# KPI Backend Recovery — Common Reprocess Contract

Estado: **DECIDED / PLANNED**

## Nombre

```text
REPROCESS_CURRENT
```

No llamarlo `DEBUG_MODE`.

## Jobs autorizados en esta secuencia

```text
KPI Runtime
Historian
```

Delivery y Timeseries reprocess NO están autorizados en esta secuencia.

## Default

```text
false
```

## Semántica

Con `false`:

```text
current checkpoint
→ comportamiento normal
→ skip
```

Con `true`:

```text
current checkpoint
→ bypass sólo del shortcut already-current
→ volver a materializar con authority upstream actual
```

## Nunca bypass

```text
source/authority regression
missing upstream authority
lease
cancellation
fencing
referential validation
write conflict detection
```

## No retroceso

```text
upstream < committed/checkpoint
→ error
```

## No source inventado

```text
upstream missing
→ no fabricar watermark
```

## Idempotencia

Contenido ya existente e idéntico:

```text
→ unchanged / convergente
```

Contenido faltante:

```text
→ rebuild
```

Contenido incompatible bajo write-once:

```text
→ conflict
```

## Ejecución controlada

Preferir:

```text
REPROCESS_CURRENT=true
--run-once
```

para repair/testing puntual.
