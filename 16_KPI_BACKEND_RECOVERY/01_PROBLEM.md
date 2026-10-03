# KPI Backend Recovery — Problem / Closure Context

Estado: **CLOSED / HISTORICAL CONTEXT**

El frente original acumuló tres problemas relacionados:

```text
1. Runtime/Historian necesitaban rematerializar CURRENT sin nueva data.
2. Delivery/Timeseries debían dejar de depender de configuración/Registry legacy.
3. Historian/Timeseries necesitaban una frontera coherente para history durable y rolling.
```

Estado final CURRENT:

```text
KPI Runtime
→ durable evaluation batches

KPI Materialization
→ Registry local per Tool

Latest Delivery
→ named connections + Registry materialized + per-Tool progress

Historian
→ durable daily history + rolling current.parquet + HistorianAuthority

Timeseries Delivery
→ Registry materialized + named connections + rolling + per-Tool checkpoints
```

La representación tabular KPI reusable se concentra dentro del mismo package:

```text
ada.kpis.history.dataset
```

No existe package separado `history-tabular`.

Los procesos no poseen conversión PyArrow directa.

Este documento conserva la motivación histórica.

No representa un gap interno OPEN.

El único pendiente operativo de este dominio es el E2E completo, actualmente BLOCKED por correcciones Web externas al backend KPI.
