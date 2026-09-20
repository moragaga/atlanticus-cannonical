# KPI Backend Recovery — Problem / Closure Context

Estado: **CLOSED / HISTORICAL CONTEXT**

El problema original era doble:

```text
1. Runtime/Historian no podían rematerializar el watermark CURRENT sin nueva data.
2. Delivery/Timeseries consumían un projection document KPI legacy distinto del KPI Registry durable CURRENT.
```

Ambos problemas están resueltos en `atlanticus@3ca8c833...`.

Runtime e Historian soportan `REPROCESS_CURRENT=false|true` con authority/fencing preservados.

Delivery y Timeseries consumen el Registry durable sin dual reader ni fallback y materializan outputs Cosmos propios.

Este documento conserva la motivación; ya no representa un gap OPEN.
