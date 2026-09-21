# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

## CLOSED — KPI backend

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

## CLOSED — Collector capability

```text
ATLANTICUS-WEB-OBSERVABILITY-SERVICE
CLOSED / VERIFIED / CURRENT

ADA-WEB-KPI-COLLECTOR-CAPABILITY
CLOSED / VERIFIED / CURRENT

KPI-COLLECTOR-DEFINITION-ATTACHMENT
CLOSED / VERIFIED / CURRENT

KPI-COLLECTOR-REAL-WEB-SMOKE
CLOSED / VERIFIED / CURRENT
```

Ya no están OPEN:

```text
exact numeric intervals
Latest/Timeseries server coherency policy
browser store ownership
component/destination mapping
poller lifecycle
Web observability integration
WebApplicationDefinition attachment
```

## OPEN — Collector operational integration

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

Pregunta operacional única:

```text
¿Dónde y cómo se compone CURRENT la aplicación operacional real que posee ToolConfiguration,
Tool projection revision y Cosmos configuration/client para poder instanciar AdaKpiCollector y
aplicar attach_ada_kpi_collector?
```

No responder desde memoria ni creando una aplicación nueva. Inspeccionar `atlanticus:main` y
usar el composition root real.

Acceptance a cerrar allí:

```text
collector realmente montado en la aplicación operacional
ToolStructure real alimenta component stores
reader usa Cosmos configuration CURRENT
health sigue sin arrancar poller
request operacional inicia poller
Latest/Timeseries llegan a stores de lectura browser
```

## OPEN — KPI Inspection stale Definition consumer

```text
KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT
OPEN / SEPARATE
```

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
