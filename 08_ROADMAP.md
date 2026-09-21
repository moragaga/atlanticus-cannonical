# Atlanticus — Roadmap

Estado: **CURRENT EXECUTION ROADMAP**

## Checkpoint publicado

```text
moragaga/atlanticus@d484569cbe0290f38f239481cde81b13a23deecf
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

## Collector capability

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

## NEXT — no perder esta frontera

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

Objetivo único: **integrar el collector ya implementado** en la composición operacional real.

No volver a discutir polling, coherency, stores ni observability salvo conflicto demostrado.

El siguiente chat debe inspeccionar la fuente autoritativa para ubicar la composición que ya
resuelve la Tool y sus conexiones. Después debe hacer el wiring mínimo:

```text
ToolConfiguration CURRENT
    ↓
ToolStructure + tool projection revision
    ↓
Cosmos client/configuration CURRENT
    ↓
CosmosKpiDeliveryReader
    ↓
AdaKpiCollector
    ↓
attach_ada_kpi_collector(existing WebApplicationDefinition, collector)
    ↓
create_web_application
```

Acceptance del siguiente foco debe comprobar la aplicación operacional real, no sólo el package
collector aislado.

## Frentes separados

```text
KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT
OPEN / SEPARATE

PYTHON-METADATA-ALIGNMENT
OPEN / SEPARATE

FULL-BACKEND-PYTEST-TOPOLOGY
BLOCKED / SEPARATE
```

No mezclar estos frentes con la integración operacional del collector.
