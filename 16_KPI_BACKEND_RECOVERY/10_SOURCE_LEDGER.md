# KPI Backend Recovery — Source Ledger

Estado: **AUDIT LEDGER / CURRENT**

## Implementation cut

```text
moragaga/atlanticus@a521e807d22451a9a4f86f11f07bcde7632b1a33
date = 2026-10-02T23:08:56Z
```

## Decisions

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

No se identificó en este cierre una decisión frozen específica que formalice todavía el rolling Historian de 24 h / 30 s o el nuevo Timeseries multi-Tool.

Clasificación:

```text
Project agreement       PROPOSED / PLANNED
decisions formalization UNVERIFIED / NOT YET OBSERVED
```

## Canonical before replacement

```text
moragaga/atlanticus-cannonical@f0de87407c59181527a22737220d14f8e0d1e309
```

El canonical previo todavía describía Latest como consumidor directo del Registry Cosmos y Timeseries como diseño cerrado a una conexión / 120 s.

Latest estaba stale respecto de main.

Timeseries continúa describiendo la implementación vigente, pero ya no representa la dirección planificada.

## Inspected CURRENT implementation

### Connections

```text
scopes/ada-kpi-engine/kpis/connections/
```

CURRENT:

```text
named Tool connections
dynamic environment variable declarations
strict tool_key
duplicate physical endpoint/database rejection
```

### Materialization

```text
scopes/ada-kpi-engine/kpis/materialization/
scopes/ada-kpi-engine/processes/kpi-materialization/
```

CURRENT:

```text
full Registry + root tool_key
per-Tool local JSON
sequential acquisition
LKG preservation on failures
30 s readiness retry for missing remote Registry
```

### Latest Delivery

```text
scopes/ada-kpi-engine/processes/kpi-delivery/
```

CURRENT:

```text
materialized Registry consumer
30 s startup readiness
process-lifetime freeze
1 s normal polling
parallel per-Tool publication
per-Tool checkpoints
partial failure isolation
```

### Historian

```text
scopes/ada-kpi-engine/processes/kpi-historian/
```

CURRENT:

```text
daily durable long history
error history
HistorianAuthority
no rolling wide Timeseries read model yet
```

### Timeseries Delivery

```text
scopes/ada-kpi-engine/processes/kpi-timeseries-delivery/
```

CURRENT implementation:

```text
direct Registry Cosmos reader
single configuration
direct durable-history scan
global checkpoint
120 s grid
```

PLANNED replacement documented separately.
