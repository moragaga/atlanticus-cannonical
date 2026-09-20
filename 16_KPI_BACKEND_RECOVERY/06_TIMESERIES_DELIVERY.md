# KPI Backend Recovery — Timeseries Delivery

Estado: **PLANNED — REGISTRY CONSUMER ALIGNMENT**

## CURRENT conflict

`kpi-timeseries-delivery` todavía consume el projection document legacy:

```text
ada_kpi_configuration_projection
```

con payload:

```text
configuration.bindings
binding.key
```

El Registry CURRENT usa:

```text
ada_kpi_registry_projection_record
payload.bindings
binding.kpi_key
```

## Cambio autorizado

```text
KPI-TIMESERIES-REGISTRY-CONSUMPTION
PLANNED
```

Timeseries debe cargar el KPI Registry durable desde Cosmos al inicio del flujo.

No dual reader.

No legacy fallback.

No copiar el Registry a otro documento sólo para Timeseries.

## Reprocess

La propuesta histórica:

```text
Timeseries Delivery REPROCESS_CURRENT
```

queda:

```text
PROPOSED / DEFERRED / NOT AUTHORIZED
```

No mezclar con el cambio de configuration consumer.

## No cambia

```text
Historian authority
aligned timeseries watermark
checkpoint authority validation
publish/checkpoint fencing
```
