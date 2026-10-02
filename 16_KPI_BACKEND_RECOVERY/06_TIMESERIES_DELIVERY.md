# KPI Backend Recovery — Timeseries Delivery

Estado: **CURRENT IMPLEMENTATION / PLANNED REPLACEMENT**

## CURRENT implementación en main

La implementación actual todavía corresponde al contrato anterior:

```text
single Cosmos connection
direct Registry read from Cosmos
single frozen KpiDeliveryConfiguration
single global checkpoint
direct scans over durable Historian Parquet
single Timeseries output
step_seconds = 120
```

Este código sigue siendo realidad implementada hasta que el reemplazo se implemente y valide.

No describir el diseño nuevo como CURRENT antes de ese cutover.

## SUPERSEDED como dirección futura

La estrategia de que Timeseries Delivery reconstruya ventanas leyendo historia durable por cada configuración queda reemplazada conceptualmente por un rolling read model producido por Historian.

## PLANNED target

START:

```text
read config/connections.json once
resolve named Cosmos connections once
wait for materialized Registries
retry readiness every 30 s
freeze per-Tool configuration for process lifetime
consolidate required series in memory
```

RUNNING:

```text
read Historian authority
validate rolling read-model coherence
read rolling wide Parquet once
hydrate requested logical grid
build snapshot per Tool
publish Tools in parallel
checkpoint each successful Tool
```

No hot reload de Registries.

## Consolidated read plan

```text
required_columns = union(series_enabled KPI keys)
max_window       = max(series_hours)
```

`series_hours` continúa limitado a `1..24`.

El plan vive en memoria.
No requiere documento durable adicional.

Timeseries debe leer el rolling una sola vez por cambio relevante, proyectando solo:

```text
timestamp_utc
+
required KPI columns
```

## Hydration

Cada Tool recibe únicamente su configuración.

Si una columna solicitada no existe:

```text
virtual column = null
```

Si falta un timestamp dentro de la grilla lógica:

```text
value = null
```

El rolling no necesita materializar filas o columnas nulas artificiales.

## Output form

Mantener salida compacta/soft.
No introducir diccionarios indexados por cada timestamp.

La representación física wide del rolling es una optimización interna y no obliga al output Cosmos a usar el mismo shape.

## Publication target

```text
parallel publication per Tool
bounded worker pool
main thread owns checkpoint/fencing/runtime mutation
partial failure preserves successful Tool progress
```

No compartir implementación de proceso con Latest por conveniencia.
Compartir solo contratos genuinamente reutilizables.

## Checkpoint target

PLANNED:

```text
per Tool
aligned/read-model watermark
registry revision
registry digest
```

## OPEN antes de implementar

```text
1. Confirm exact Timeseries output step_seconds under the new 30 s rolling grid.
2. Freeze rolling metadata/coherence contract with HistorianAuthority.
3. Freeze per-Tool checkpoint payload and state key.
4. Decide whether Timeseries coalesces directly to latest available rolling watermark
   when multiple intermediate grids were missed.
5. Confirm whether output schema_version remains 2 or requires a new schema.
```

Hasta resolver estos puntos, Timeseries permanece `PLANNED`.
