# Atlanticus — Roadmap

Estado: **CURRENT EXECUTION ROADMAP**

## Checkpoint publicado

```text
moragaga/atlanticus@3ca8c833df916a4e0812c76eaba84ee5fde8a1cc
```

## KPI backend flow

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

## NEXT

```text
ADA-GENERIC-COLLECTOR-CLOSURE
PLANNED / NEXT
```

Objetivo del siguiente foco: cerrar el mapping real desde outputs KPI materializados hacia los stores de lectura de ADA Generic, usando el contrato Tool CURRENT.

Restricciones ya decididas:

```text
Latest priority > Timeseries priority
Latest and Timeseries use different load intervals
no new legacy contract
no shared implementation invented without a real boundary
```

A resolver allí, después de inspección:

```text
exact polling/load intervals
read synchronization semantics
existing UI output stores and their contracts
exact Tool component/destination mapping
minimal code delta
```

## Frentes separados

```text
KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT
OPEN / SEPARATE

PYTHON-METADATA-ALIGNMENT
OPEN / SEPARATE

FULL-BACKEND-PYTEST-TOPOLOGY
BLOCKED / SEPARATE
```
