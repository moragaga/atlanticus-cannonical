# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

## CLOSED — KPI backend recovery and Registry consumption

```text
KPI-RUNTIME-REPROCESS-CURRENT
CLOSED / VERIFIED / CURRENT

KPI-DELIVERY-REGISTRY-CONSUMPTION
CLOSED / VERIFIED / CURRENT

KPI-TIMESERIES-REGISTRY-CONSUMPTION
CLOSED / VERIFIED / CURRENT

KPI-HISTORIAN-REPROCESS-CURRENT
CLOSED / VERIFIED / CURRENT
```

No reabrir estos contracts salvo conflicto demostrado.

## OPEN — ADA Generic Collector

```text
ADA-GENERIC-COLLECTOR-CLOSURE
PLANNED / NEXT
```

Ya no está bloqueado por KPI backend.

Decidido:

```text
Latest y Timeseries son superficies de lectura independientes.
Latest es prioritario.
Deben soportar intervalos de carga distintos.
Tools aporta el contrato estructural/destination necesario para el mapping.
```

OPEN:

```text
exact numeric intervals
sync/coherency semantics between Latest and Timeseries reads
existing store ownership in ADA Generic
whether existing reader/collector code already satisfies the boundary
minimal integration location
```

## OPEN — KPI Inspection stale Definition consumer

```text
KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT
OPEN / SEPARATE
```

No mezclar con Collector salvo dependencia directa demostrada.

## OPEN — Python metadata

```text
Project baseline = Python 3.14.7
some package metadata observed = 3.14.2
PYTHON-METADATA-ALIGNMENT = OPEN / SEPARATE
```

## BLOCKED / SEPARATE — full backend test topology

```text
collection failures around tests.support
UNVERIFIED AS PREEXISTING
```

## PROPOSED / DEFERRED

```text
Latest Delivery REPROCESS_CURRENT
Timeseries Delivery REPROCESS_CURRENT
Historian reprocess_from optimization
```
