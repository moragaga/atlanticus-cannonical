# Atlanticus — Roadmap

Estado: **CURRENT EXECUTION ROADMAP**

## Regla

Un solo foco por incremento.

Cerrar cada frontera con evidencia suficiente.

No conservar legacy para sostener consumers o tests anteriores.

## Checkpoint publicado

```text
moragaga/atlanticus@d71e94d12fa31a986b3ecc0262fbbb6ef2e4a3dd
```

## Hitos KPI cerrados

```text
KPI-REGISTRY-CAPABILITY-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-CAPABILITY-CUTOVER
CLOSED / VERIFIED / CURRENT
```

La precondición Web/durable para recovery y configuración backend ya está disponible.

## Secuencia NEXT

### 1. KPI Runtime

```text
KPI-RUNTIME-REPROCESS-CURRENT
PLANNED / NEXT
```

Agregar `REPROCESS_CURRENT=false` y permitir reevaluar exactamente el watermark current sin
relajar authority, lease, fencing o conflict checks.

### 2. Delivery + Timeseries

```text
KPI-DELIVERY-REGISTRY-CONSUMPTION
PLANNED

KPI-TIMESERIES-REGISTRY-CONSUMPTION
PLANNED
```

Reemplazar consumo del documento legacy `ada_kpi_configuration_projection` por el KPI Registry
durable CURRENT desde Cosmos.

No dual reader.

No schema legacy fallback.

### 3. Historian

```text
KPI-HISTORIAN-REPROCESS-CURRENT
PLANNED
```

Forced-current relee todos los evaluation batches durables desde inicio hasta KPI committed.

### 4. Collector

```text
ADA-GENERIC-COLLECTOR-CLOSURE
BLOCKED / AFTER KPI BACKEND FLOW
```

Cerrar sólo después de conocer y calificar el extremo final del flujo KPI.

## Frentes separados

```text
KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT
OPEN / SEPARATE

PYTHON-METADATA-ALIGNMENT
OPEN / SEPARATE

remote CI
UNVERIFIED

full workspace qualification
UNVERIFIED
```

## No incluir en la secuencia autorizada

```text
Latest Delivery REPROCESS_CURRENT
PROPOSED / DEFERRED

Timeseries Delivery REPROCESS_CURRENT
PROPOSED / DEFERRED
```
