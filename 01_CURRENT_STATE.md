# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

```text
Implementation
moragaga/atlanticus@3ca8c833df916a4e0812c76eaba84ee5fde8a1cc

Parent
06e8f4ba4882c2d2600f995cce00b7bfdc01990c

Tree
b53495d710ae9307ce5b64da3311880d4bd6c050

Canonical inspected before replacement
moragaga/atlanticus-cannonical@961447d3a1b3d2afaff7da148f85729fdbe4beab
```

Git permanece SOLO LECTURA.

## Estado resumido

```text
KPI-REGISTRY-CAPABILITY-CUTOVER                 CLOSED / VERIFIED / CURRENT
KPI-DEFINITION-CAPABILITY-CUTOVER               CLOSED / VERIFIED / CURRENT
KPI-RUNTIME-REPROCESS-CURRENT                   CLOSED / VERIFIED / CURRENT
KPI-DELIVERY-REGISTRY-CONSUMPTION               CLOSED / VERIFIED / CURRENT
KPI-TIMESERIES-REGISTRY-CONSUMPTION             CLOSED / VERIFIED / CURRENT
KPI-HISTORIAN-REPROCESS-CURRENT                 CLOSED / VERIFIED / CURRENT

ADA-GENERIC-COLLECTOR-CLOSURE                   PLANNED / NEXT

KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT  OPEN / SEPARATE
PYTHON-METADATA-ALIGNMENT                       OPEN / SEPARATE
FULL-BACKEND-PYTEST-TOPOLOGY                    BLOCKED / UNVERIFIED AS PREEXISTING / SEPARATE
```

## KPI Runtime CURRENT

`REPROCESS_CURRENT=false` conserva `observed == committed -> up_to_date`.

`REPROCESS_CURRENT=true` sólo fuerza el watermark CURRENT. Para el mismo watermark durable reutiliza el `evaluated_at_utc` ya persistido; por tanto:

```text
same watermark + same results
→ write_once UNCHANGED

same watermark + changed results
→ durable conflict

committed batch missing
→ explicit data error

observed < committed
→ rejected
```

Lease, cancellation, fencing y authority ordering permanecen intactos.

## Delivery CURRENT

Consume directamente el KPI Registry durable desde Cosmos mediante reader propio del proceso.

```text
input container = ada-kpi-registry-projection
partition path  = /partition_key
partition value = kpis
document_type   = ada_kpi_registry_projection_record
schema_version  = 1
```

No existe reader legacy ni fallback.

Output owned:

```text
container      = ada-kpi-latest-delivery
partition path = /partition_id
TTL            = None
id             = latest
partition_id   = kpis
document_type  = ada_kpi_latest_delivery
schema_version = 1
```

El proceso valida el Registry consumido y asegura idempotentemente su output container una vez al startup.

## Timeseries Delivery CURRENT

Consume el mismo KPI Registry durable mediante reader independiente.

Output owned:

```text
container      = ada-kpi-timeseries-delivery
partition path = /partition_id
TTL            = None
id             = timeseries
partition_id   = kpis
document_type  = ada_kpi_timeseries_delivery
schema_version = 2
step_seconds   = 120
```

El proceso valida el Registry consumido y asegura idempotentemente su output container una vez al startup.

## Container configuration CURRENT

Variables de conexión/database permanecen externas:

```text
COSMOS_CONSUMPTION_ENDPOINT
COSMOS_CONSUMPTION_KEY
COSMOS_CONSUMPTION_DATABASE_NAME
```

La identidad/topología de containers ya no se configura por ENV.

```text
container name
partition key path
partition value
TTL
document_type
schema_version
item identity
```

son contratos internos de cada proceso.

## Historian CURRENT

`REPROCESS_CURRENT=false` conserva `authority == committed -> SKIPPED_CURRENT`.

Con `REPROCESS_CURRENT=true` y authority CURRENT:

```text
after = None
through = KPI committed
→ replay de todos los durable evaluation batches
→ rematerialize history/errors
→ commit misma authority
```

Si Historian está atrasado, incluso con el flag true conserva catch-up incremental. Authority ahead of KPI continúa siendo error.

## Collector

La cadena backend KPI está cerrada, por tanto:

```text
ADA-GENERIC-COLLECTOR-CLOSURE
PLANNED / NEXT
```

Decidido para el siguiente foco:

```text
Latest y Timeseries usan intervalos de carga distintos.
Latest es prioritario y debe refrescarse con mayor prioridad/frecuencia que Timeseries.
```

OPEN para diseño/verificación en el siguiente chat:

```text
valores exactos de intervalos
mecanismo de sincronización de lecturas
mapeo a los stores de salida de la UI
uso exacto del contrato Tool CURRENT para component/destination binding
```

No inventar esos detalles antes de inspeccionar `atlanticus:main`.
