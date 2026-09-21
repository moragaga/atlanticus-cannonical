# ADA Web — Current Baseline

Estado: **CURRENT**

## Implementación auditada

```text
moragaga/atlanticus@d484569cbe0290f38f239481cde81b13a23deecf
```

## KPI Registry

```text
scopes/ada/web/kpis/registry/
├── core
├── configuration
├── projection-local
└── projection-cosmos
```

Status:

```text
CLOSED / VERIFIED / CURRENT
```

## KPI Definition

```text
scopes/ada/web/kpis/definition/
├── core
├── configuration
├── projection-local
└── projection-cosmos
```

Status:

```text
CLOSED / VERIFIED / CURRENT
```

## KPI Collector

```text
scopes/ada/web/kpis/collector
ada-web-kpi-collector==0.1.0
```

Core public capability includes:

```text
AdaKpiCollector
CosmosKpiDeliveryReader
AdaKpiCollectorPollingRuntime
AdaKpiCollectorWebIntegration
attach_ada_kpi_collector
component_kpi_store_id
resolve_kpi_collector_browser_update
```

Defaults:

```text
Latest polling      10 s
Timeseries polling 120 s
Browser refresh     10 s
```

One Component Store per Tool Component.

## Atlanticus Web observability service

Public service key:

```text
WEB_OBSERVABILITY_SERVICE_KEY = atlanticus.web.observability
```

`create_web_application()` registers the runtime-owned `WebObservability` instance before module
service registration and freezes the registry after modules register.

Collector declares this service requirement and uses the same runtime observability instance.

## Collector Web lifecycle

```text
health/assets/auth infrastructure request
→ poller remains stopped

real application request
→ per-worker poller starts
→ refresh happens outside request thread
```

The browser callback reads only in-process snapshot state.

## Generic Application compatibility

Generic Application remains valid without Collector.

Qualification after Collector closure:

```text
scripts/scopes/ada/check.sh application
63 passed
```

## Python

Project baseline:

```text
3.14.7
```

Some package metadata remains observed at:

```text
requires-python ==3.14.2
```

Classification:

```text
PYTHON-METADATA-ALIGNMENT
OPEN / SEPARATE
```

## Separate conflict

KPI Inspection Definition provider still consumes a historical Definition contract.

It does not belong to Collector operational integration unless a direct dependency is later
demonstrated.
