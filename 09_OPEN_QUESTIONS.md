# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contracts CLOSED.

## CLOSED — KPI Registry

```text
KPI-REGISTRY-CAPABILITY-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No reabrir:

```text
KpiRegistry naming
SourceKey('kpis')
core/configuration/projection-local/projection-cosmos structure
Tool exact ProjectionTarget dependency
Cosmos storage contract
```

## CLOSED — KPI Definition

```text
KPI-DEFINITION-CAPABILITY-CUTOVER
CLOSED / VERIFIED / CURRENT
```

No reabrir:

```text
KpiDefinition domain
SourceKey('kpi-definitions')
core/configuration/projection-local/projection-cosmos structure
Registry exact ProjectionTarget dependency
Cosmos storage contract
```

## OPEN — KPI Runtime recovery

```text
KPI-RUNTIME-REPROCESS-CURRENT
PLANNED / NEXT
```

Debe implementar únicamente el bypass de `observed == committed` con `REPROCESS_CURRENT=true`.

## OPEN — Delivery Registry consumption

```text
KPI-DELIVERY-REGISTRY-CONSUMPTION
PLANNED
```

CURRENT backend todavía consume `ada_kpi_configuration_projection`.

Debe migrar al Registry durable sin dual reader.

## OPEN — Timeseries Registry consumption

```text
KPI-TIMESERIES-REGISTRY-CONSUMPTION
PLANNED
```

Mismo conflicto de contrato que Delivery.

## OPEN — Historian recovery

```text
KPI-HISTORIAN-REPROCESS-CURRENT
PLANNED
```

Forced-current requiere full replay hasta committed.

## BLOCKED — ADA Generic Collector

```text
ADA-GENERIC-COLLECTOR-CLOSURE
BLOCKED
```

Espera cierre de la cadena backend KPI.

## OPEN — KPI Inspection stale Definition consumer

```text
KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT
OPEN / SEPARATE
```

CURRENT aún declara `ada-web-kpi-definition==0.1.0` y consume un repository contract histórico.

No resolver durante backend recovery salvo que se demuestre dependencia directa.

## OPEN — Python metadata

```text
Project baseline
3.14.7

observed KPI/Web package metadata
==3.14.2
```

Mantener separado.

## PROPOSED / DEFERRED

```text
Latest Delivery REPROCESS_CURRENT
Timeseries Delivery REPROCESS_CURRENT
reprocess_from optimization
```

No autorizados en la secuencia actual.
