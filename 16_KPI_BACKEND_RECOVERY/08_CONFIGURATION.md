# KPI Backend Reprocessing — Configuration

Estado: **CURRENT DIRECTION / BASELINE 1.0**

Cada job tiene su propia configuración y se despliega/ejecuta de forma independiente.

Por tanto no se necesita un nombre ENV diferente por proceso.

## Variable común

```text
REPROCESS_CURRENT=false
```

La misma variable existe dentro del contrato de cada job que soporte reproceso.

Ejemplos:

```text
kpi-runtime
  REPROCESS_CURRENT=true

kpi-historian
  REPROCESS_CURRENT=true
```

No existe un flag global que afecte simultáneamente todos los procesos.

## Semántica

```text
false
→ comportamiento productivo normal

true
→ omite únicamente el shortcut "already current"
```

No se mezcla con:

- DEBUG;
- logging;
- observability;
- run_once;
- poll interval.

## Ejecución controlada

Para repair/testing puntual se recomienda:

```text
REPROCESS_CURRENT=true
+
--run-once
```

cuando corresponda.

Si un job continuo mantiene la variable en true, reprocesará current en cada ciclo; esa conducta es explícita y no se auto-resetea desde código.
