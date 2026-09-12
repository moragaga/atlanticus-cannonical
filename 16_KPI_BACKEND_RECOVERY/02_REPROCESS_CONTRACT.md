# KPI Backend Reprocessing — Common Contract

Estado: **PROPOSED**

## Nombre conceptual

`REPROCESS_CURRENT`

No llamarlo `DEBUG_MODE`.

Debug describe observabilidad/desarrollo.

Este contrato describe:

> ignorar únicamente el shortcut de "ya está current" y volver a materializar usando la authority upstream actual.

## Default

```text
false
```

Siempre.

Producción normal no cambia.

## Semántica transversal

Con `REPROCESS_CURRENT=false`:

```text
current checkpoint
→ skip
```

Con `REPROCESS_CURRENT=true`:

```text
current checkpoint
→ ejecutar nuevamente
```

Pero se conservan:

- source authority;
- watermark ordering;
- committed watermark;
- lease checks;
- cancellation;
- fencing;
- referential/config validation;
- write conflict detection.

## No retroceso

Nunca permite:

```text
upstream watermark < committed/checkpoint
→ continuar
```

Los errores de regresión siguen siendo errores.

## No source inventado

No permite:

```text
source watermark missing
→ fabricar watermark
```

El reproceso trabaja con authority existente.

## Idempotencia

Si el materializado todavía existe y su contenido es idéntico:

```text
reprocess
→ unchanged / merge idempotente
```

Si falta:

```text
reprocess
→ rebuild
```

Si existe contenido durable incompatible donde el contrato exige write-once:

```text
→ conflict
```

No sobrescribir silenciosamente.
