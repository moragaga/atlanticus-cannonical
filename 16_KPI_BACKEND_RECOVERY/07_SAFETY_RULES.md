# KPI Backend Recovery — Safety Rules

Estado: **CURRENT**

`REPROCESS_CURRENT` nunca significa:

```text
ignore errors
ignore authority
move watermark backwards
overwrite durable conflicts
disable lease
disable fencing
disable cancellation
fabricate upstream data
```

Default siempre false.

Para repair puntual:

```text
REPROCESS_CURRENT=true
+
--run-once
```

No auto-reset del flag dentro del proceso.

Delivery/Timeseries no recibieron reprocess en este cierre.
