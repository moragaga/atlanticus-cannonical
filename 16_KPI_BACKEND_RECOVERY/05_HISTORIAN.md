# KPI Backend Recovery — Historian

Estado: **CLOSED / VERIFIED / CURRENT**

## Autoridad implementada

Corte verificado:

```text
moragaga/atlanticus@38bcd8c5607d67f999e2bc4bf9dbf176c8340588
```

Historian mantiene dos superficies con responsabilidades distintas:

```text
daily long history
    = durable authority

rolling wide parquet
    = regenerable/read-optimized Timeseries projection
```

`REPROCESS_CURRENT`, error history y `KpiHistorianAuthority` permanecen CURRENT.

## Flujo CURRENT

```text
KPI evaluation batches
    ├─> daily durable long history
    ├─> rolling Timeseries projection
    └─> HistorianAuthority commit
```

Orden contractual:

```text
durable history write
→ rolling update
→ HistorianAuthority commit
```

Si durable history falla, no se publica rolling ni authority.

Si rolling falla, no se avanza `HistorianAuthority`.

## Ruta física congelada

No existe variable de entorno nueva para el rolling.

La ruta deriva de `application_root`:

```text
<application_root>/timeseries/current.parquet
```

`datasets/` continúa siendo la frontera durable administrada por `DatasetRuntime`.

`timeseries/` es una proyección operacional distinta, descartable y regenerable.

## Contrato compartido

`ada.kpis.history.rolling` expone:

```text
ROLLING_SCHEMA_VERSION   = 1
ROLLING_GRID_SECONDS     = 30
ROLLING_MAX_HOURS        = 24
ROLLING_DIRECTORY        = "timeseries"
ROLLING_FILENAME         = "current.parquet"
ROLLING_METADATA_KEY     = "ada_kpi_timeseries"
ROLLING_TIMESTAMP_COLUMN = "timestamp_utc"

ROLLING_VALUE_TYPES:
text
integer
float
boolean
```

El rolling no acepta JSON como serie escalar.

## Schema físico Parquet

Forma wide:

```text
timestamp_utc
<kpi_key_1>
<kpi_key_2>
...
```

Contrato:

```text
timestamp_utc = Arrow timestamp(us, UTC), non-null
KPI columns   = nullable string
column order  = KPI keys sorted
```

Los valores escalares físicos permanecen en su representación string canónica.

No se persisten filas artificiales para completar la ventana lógica.

## Metadata exacta

La metadata JSON canónica se almacena bajo:

```text
ada_kpi_timeseries
```

Payload:

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

`value_types` es un mapping:

```text
kpi_key -> text | integer | float | boolean
```

Invariantes:

```text
watermark_utc       aligned to 30 s
coverage_start_utc  aligned to 30 s when present
coverage_end_utc    aligned to 30 s when present
historian_revision  exact revision derived from watermark_utc
coverage start/end  both present or both null
coverage_start      <= coverage_end <= watermark
coverage_start      strictly inside (watermark - 24 h, watermark]
```

Cobertura vacía implica:

```text
coverage_start_utc = null
coverage_end_utc   = null
value_types        = {}
```

Cobertura física no vacía requiere `value_types` no vacío.

## Grilla y alineación

Historian valida alineación estricta a 30 segundos.

No redondea ni hace floor silencioso.

Un watermark o punto procesado fuera de la grilla contractual falla con
`KpiHistorianRollingError`.

## Semántica de cobertura

El archivo almacena solo datos observados y utilizables.

```text
missing timestamp   = la fila física puede no existir
missing KPI value   = celda ausente/null
missing/error point = no valor escalar usable
```

No se fabrican 24 horas de filas nulas.

La ventana física se conserva en:

```text
(watermark - 24 h, watermark]
```

Los puntos en o antes del cutoff se eliminan.

## Cambios de tipo

Si un KPI cambia de `value_type` escalar dentro del horizonte rolling:

```text
old physical values for that KPI are cleared
new scalar type starts a new logical series
```

Si aparece un resultado JSON:

```text
that KPI rolling scalar series is cleared/excluded
```

Un valor escalar posterior puede iniciar nuevamente una serie escalar.

## Incremental path

En operación normal Historian no relee durable history para actualizar el rolling.

```text
new batches already held by Historian
→ merge into current rolling
→ trim horizon
→ atomic replace
```

El merge es idempotente por timestamp/KPI para reintentos del mismo rango.

## Recovery path

Si el rolling está ausente, corrupto o no es coherente con la authority previa:

```text
durable history exists
→ read only the dates needed to cover the last 24 h
→ rebuild wide rolling
→ atomic replace
```

Si todavía no existe history durable previa:

```text
current batches
→ create rolling
→ physical coverage grows naturally
```

No se crea backfill artificial de `null`.

## Coherencia

`KpiHistorianRollingMaterializer.is_coherent(authority)` valida metadata y revisión.

Casos:

```text
rolling == authority
→ coherent

rolling missing/corrupt/behind
→ not coherent; rebuild when required

rolling ahead of authority
→ error
```

Cuando Historian ya está CURRENT y no se solicita reprocess:

```text
rolling coherent
→ SKIPPED_CURRENT

rolling incoherent/missing/corrupt
→ rebuild rolling from durable history
→ PROCESSED
```

Ese recovery no reescribe durable history ni recommitea `HistorianAuthority`.

## Atomicidad

La escritura usa un archivo temporal sibling en el mismo directorio y reemplazo mediante
`os.replace`.

Contrato validado:

```text
failed new write
→ previously committed rolling remains intact
```

## Boundary congelado

Historian no conoce:

```text
Tools
destination_keys
named output Cosmos connections
Timeseries Delivery checkpoints
logical per-Tool windows
```

La hidratación de una grilla lógica completa pertenece al consumidor Timeseries Delivery.

## Qualification

Validación final reportada sobre el árbol integrado:

```text
kpis/history
27 passed
ruff check                 PASS
ruff format --check        PASS

processes/kpi-historian
47 passed
ruff check                 PASS
ruff format --check        PASS

git diff --check
PASS
```

Cobertura relevante validada:

```text
shared metadata roundtrip and validation
exact rolling path
incremental update without durable reread
observed rows only
empty physical coverage
value_type transition
24 h trimming
durable rebuild
corrupt rolling recovery
strict 30 s alignment
atomic replacement failure preservation
history -> rolling -> authority ordering
current coherent skip
current incoherent recovery
composition/public API
```

## OPEN fuera de este cierre

No queda un contrato interno del Historian bloqueando el siguiente incremento.

Permanece UNVERIFIED fuera de este foco:

```text
full operational E2E with the future Timeseries consumer
real production storage/runtime behavior
load/performance profile
```

Esas validaciones no reabren el contrato CURRENT del rolling.
