# KPI Backend Recovery — Historian

Estado: **CLOSED / VERIFIED / CURRENT**

## Autoridad implementada

Corte actual:

```text
moragaga/atlanticus@2505196019fcc51e5f97ff66a3159beb87fe71f0
```

Historian mantiene dos superficies distintas:

```text
daily long history
    = durable authority

rolling wide dataset
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
durable history
→ rolling
→ HistorianAuthority
```

Durable failure impide rolling y authority.

Rolling failure impide authority.

## Ruta rolling congelada

```text
<application_root>/timeseries/current.parquet
```

No existe variable de entorno adicional para esa ruta.

## Contrato lógico y representación tabular

El package reusable único es:

```text
ada-kpis-history
```

Separación CURRENT:

```text
ada.kpis.history.contract
    durable DatasetDefinition / targets / key and ordering contract

ada.kpis.history.rolling
    KpiRollingMetadata
    grid
    horizon
    revision/coherence invariants

ada.kpis.history.dataset
    durable history Arrow schemas
    records -> Arrow table
    Arrow table -> projected durable records
    rolling DatasetDefinition / target
    rolling Arrow encode/decode/projection
```

PyArrow está aislado en:

```text
ada.kpis.history.dataset
```

Historian process no importa PyArrow directamente.

No existe package separado `ada-kpis-history-tabular`.

## Backend I/O boundary

Historian opera mediante:

```text
DatasetRuntime
```

Concrete composition:

```text
durable history
DatasetRuntime(
    ParquetDatasetStore(<application_root>/datasets)
)

rolling
DatasetRuntime(
    ParquetDatasetStore(<application_root>)
)
```

Historian no hace directamente:

```text
Parquet read/write
temporary file management
os.replace
filesystem publication
```

La atomicidad física pertenece al datasets backend.

## Rolling contract

```text
ROLLING_SCHEMA_VERSION   = 1
ROLLING_GRID_SECONDS     = 30
ROLLING_MAX_HOURS        = 24
ROLLING_DIRECTORY        = timeseries
ROLLING_FILENAME         = current.parquet
ROLLING_METADATA_KEY     = ada_kpi_timeseries
ROLLING_TIMESTAMP_COLUMN = timestamp_utc
```

Scalar value types:

```text
text
integer
float
boolean
```

JSON no forma una serie scalar rolling.

## Physical shape

Wide:

```text
timestamp_utc
<kpi_key_1>
<kpi_key_2>
...
```

Contract:

```text
timestamp_utc = Arrow timestamp(us, UTC), non-null
KPI columns   = nullable string
physical rows = observed usable points only
```

No se materializan filas artificiales para completar 24 h.

## Metadata

Canonical metadata under:

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

Invariants:

```text
watermark aligned 30 s
coverage aligned 30 s
coverage start/end both present or both null
coverage_start <= coverage_end <= watermark
coverage_start strictly inside (watermark - 24 h, watermark]
historian_revision derived from watermark
empty physical coverage => empty value_types
```

## Incremental behavior

Normal path:

```text
new batches already available to Historian
→ update current rolling state
→ trim >24 h
→ DatasetRuntime.replace
```

Durable history is not reread for normal incremental updates.

## Recovery behavior

Missing/corrupt/behind rolling:

```text
durable history
→ scan only dates required by last 24 h
→ rebuild rolling
→ DatasetRuntime.replace
```

Rolling ahead of authority is an error.

CURRENT + coherent rolling is skipped.

CURRENT + incoherent/missing/corrupt rolling is rebuilt from durable history without rewriting durable history.

## Type transitions

Scalar type change:

```text
clear previous physical series for that KPI
start new scalar series with new type
```

JSON result:

```text
clear/exclude scalar rolling series
```

A later scalar result may start a scalar series again.

## Frozen boundary

Historian does not know:

```text
Tools
destination_keys
named output Cosmos connections
Timeseries checkpoints
per-Tool logical windows
Timeseries output grid
```

## Qualification

Final focused qualification:

```text
kpis/history
31 passed

processes/kpi-historian
45 passed

Ruff check
PASS

Ruff format --check
PASS

git diff --check
PASS
```

## OPEN outside this contract

```text
full operational Runtime -> Historian -> Timeseries E2E
real production storage behavior
load/performance profile
```

These are UNVERIFIED/BLOCKED operational validations, not internal Historian design gaps.
