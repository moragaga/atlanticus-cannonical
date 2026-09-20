# KPI Backend Recovery — Configuration

Estado: **DECIDED / BASELINE 1.0**

Cada job tiene configuración independiente.

## REPROCESS_CURRENT

Variable común sólo para jobs que hayan sido autorizados a soportarla:

```text
REPROCESS_CURRENT=false
```

Autorizados ahora:

```text
kpi-runtime
kpi-historian
```

No autorizados todavía:

```text
kpi-delivery
kpi-timeseries-delivery
```

## Semántica

```text
false
→ comportamiento productivo normal

true
→ bypass exclusivamente del shortcut already-current
```

No se mezcla con:

```text
DEBUG
logging
observability
run_once
poll interval
```

## Ejecución controlada

Para repair/testing puntual:

```text
REPROCESS_CURRENT=true
+
--run-once
```

cuando corresponda.

## Registry consumer settings

Delivery y Timeseries deben migrar sus settings desde el contrato histórico de:

```text
KPI_DELIVERY_CONFIGURATION_CONTAINER
KPI_DELIVERY_CONFIGURATION_ITEM_ID
KPI_DELIVERY_CONFIGURATION_PARTITION_KEY
```

hacia el resource/identity real del KPI Registry durable CURRENT.

La forma exacta de settings debe partir del storage contract y composition CURRENT.

No inventar un segundo Registry contract.
