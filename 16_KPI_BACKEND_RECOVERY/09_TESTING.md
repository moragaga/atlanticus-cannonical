# KPI Backend Recovery — Testing

Estado: **CURRENT PLAN**

Tests de comportamiento, no de existencia de flags/functions.

## KPI Runtime

```text
current + flag false
→ skip

current + flag true + missing batch
→ rebuild

current + flag true + same batch
→ idempotent

current + flag true + conflicting batch
→ error

source regression + flag true
→ error

source missing + flag true
→ no fabrication
```

## Delivery Registry consumption

Probar:

```text
valid Registry projection document
→ configuration effective correcta

wrong document_type/schema
→ error

missing Registry projection
→ explicit error

binding kpi_key/destinations/latest/series semantics preserved

legacy ada_kpi_configuration_projection
→ not accepted by CURRENT consumer
```

No crear un test cuya única finalidad sea comprobar un import concreto o la existencia de una
clase.

## Timeseries Registry consumption

Mismo contrato Registry que Delivery.

Probar además que series settings (`series_enabled`, `series_hours`) llegan intactos al
comportamiento de Timeseries.

## Historian

```text
current + flag false
→ skip

current + flag true
→ read from beginning through committed

deleted history
→ rebuilt

existing history
→ convergent/idempotent merge

authority ahead of KPI
→ error
```

## Common

```text
cancellation respected
lease respected
fencing respected
default false
facts distinguish forced execution when contract decides expose them
```

## No incluido

No agregar tests de Delivery/Timeseries reprocess mientras esa capacidad siga no autorizada.
