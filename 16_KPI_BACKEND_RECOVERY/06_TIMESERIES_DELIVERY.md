# KPI Backend Recovery — Timeseries Delivery

Estado: **CURRENT LEGACY IMPLEMENTATION / PLANNED NEXT REPLACEMENT**

## CURRENT implementación en main

La implementación vigente aún corresponde al contrato anterior:

```text
single Cosmos connection
direct Registry read from Cosmos
single frozen KpiDeliveryConfiguration
single global checkpoint
direct scans over durable Historian Parquet
single Timeseries output
step_seconds = 120
```

Esta implementación sigue siendo realidad hasta que el reemplazo se implemente y valide.

No describir el diseño nuevo como CURRENT antes del cutover.

## SUPERSEDED como dirección futura

Queda SUPERSEDED la estrategia futura de reconstruir cada ventana Timeseries leyendo directamente
la historia durable.

El consumidor nuevo debe usar el rolling CURRENT producido por Historian.

## Upstream CURRENT / congelado

Historian ya publica:

```text
<application_root>/timeseries/current.parquet
```

Contrato disponible:

```text
schema_version      = 1
grid_seconds        = 30
max_hours           = 24
shape               = wide
timestamp_utc       = timestamp(us, UTC)
metadata key        = ada_kpi_timeseries
write               = atomic replacement
physical coverage   = observed only
```

Metadata:

```text
schema_version
watermark_utc
historian_revision
grid_seconds
max_hours
coverage_start_utc
coverage_end_utc
value_types
```

Timeseries debe validar coherencia exacta contra `HistorianAuthority`.

No debe consumir una vista adelantada, corrupta o contractualmente incompatible.

## Registry CURRENT / congelado

El Registry materializado expone por KPI:

```text
kpi_key
destination_keys
latest_enabled
series_enabled
series_hours
```

Invariante:

```text
series_enabled = true  → series_hours in 1..24
series_enabled = false → series_hours = None
```

El reemplazo de Timeseries debe consumir el Registry materializado y no volver a leer el Registry
operacional directamente desde Cosmos.

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
read HistorianAuthority
validate rolling metadata/revision coherence
read rolling wide Parquet once per relevant watermark
hydrate requested logical grid
build snapshot per Tool
publish Tools with bounded parallelism
checkpoint each successful Tool
```

No hot reload de Registries.

## Consolidated read plan

```text
required_columns = union(series_enabled KPI keys)
max_window       = max(series_hours)
```

`series_hours` permanece limitado a `1..24`.

El plan vive en memoria.

No requiere documento durable adicional.

Timeseries debe proyectar únicamente:

```text
timestamp_utc
+
required KPI columns
```

## Hydration

Cada Tool recibe únicamente su configuración y ventana.

El rolling almacena solo cobertura física observada.

El consumidor debe construir la grilla lógica requerida.

Si una columna solicitada no existe:

```text
virtual column = null
```

Si falta un timestamp dentro de la grilla lógica:

```text
value = null
```

El consumidor no debe exigir que Historian materialice filas o columnas nulas artificiales.

## Output form

Mantener salida compacta/soft.

No introducir diccionarios indexados por cada timestamp salvo que una necesidad contractual real lo
exija.

La forma física wide del rolling es una optimización interna y no obliga al documento de salida
Cosmos a adoptar el mismo shape.

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

Debe reemplazarse el checkpoint global por progreso independiente por Tool.

Shape mínimo todavía por congelar:

```text
Tool identity
delivered/read-model watermark
Registry revision
Registry digest
```

El state key exacto y el payload final permanecen OPEN.

## OPEN antes de implementar

Historian ya resolvió y cerró:

```text
rolling filesystem path
rolling Parquet schema
rolling metadata
30 s physical grid
rolling/HistorianAuthority coherence
atomic replacement
recovery behavior
```

Quedan OPEN exclusivamente en Timeseries Delivery:

```text
1. Freeze exact logical/output step_seconds.
2. Freeze per-Tool checkpoint payload and state key.
3. Decide whether delivery coalesces directly to the latest coherent rolling watermark
   after missing intermediate grids.
4. Confirm whether Timeseries output schema_version remains 2 or requires a new schema.
5. Confirm exact publication/fencing behavior when one Tool fails and others succeed,
   reusing the already-agreed independent-progress principle.
```

## Siguiente incremento

Este es el único foco recomendado:

```text
KPI-TIMESERIES-MULTI-TOOL-DELIVERY
```

Primero cerrar los contratos OPEN anteriores.

Después implementar el consumidor.

No modificar Historian en el mismo incremento salvo finding real de incompatibilidad con su
contrato CURRENT.
