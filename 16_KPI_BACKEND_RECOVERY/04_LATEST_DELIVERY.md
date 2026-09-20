# KPI Backend Recovery — Latest Delivery

Estado: **PLANNED — REGISTRY CONSUMER ALIGNMENT**

## CURRENT conflict

`kpi-delivery` todavía consume:

```text
document_type = ada_kpi_configuration_projection
configuration.bindings
binding.key
revision
tool_projection_revision
```

KPI Registry CURRENT publica:

```text
document_type = ada_kpi_registry_projection_record
payload.bindings
binding.kpi_key
source_release_id
dependencies
```

## Cambio autorizado en la secuencia actual

```text
KPI-DELIVERY-REGISTRY-CONSUMPTION
PLANNED
```

Delivery debe cargar desde Cosmos el KPI Registry durable al inicio del flujo y dejar de usar el
projection document legacy.

No:

```text
dual reader
legacy fallback
compatibility alias
secondary backend configuration projection
```

La configuración efectiva de Delivery debe derivarse desde el contrato Registry CURRENT dentro
de la frontera del consumer existente.

No inventar otra authority.

## Reprocess

La propuesta histórica:

```text
Latest Delivery REPROCESS_CURRENT
```

queda:

```text
PROPOSED / DEFERRED / NOT AUTHORIZED
```

No implementarla junto con el Registry cutover.

## Authority

Delivery continúa sin poder liderar KPI committed watermark.

El cambio de configuración no altera checkpoint/fencing/authority semantics existentes.
