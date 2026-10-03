# ADA Command Center — Engine and Projections

Estado: **CURRENT — MATERIALIZATION, RUNTIME, MODELER, DELIVERY Y LIVE PROJECTION IMPLEMENTADOS**

Checkpoint:

```text
atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856
```

## Configuration plane CURRENT

```text
AlarmConfigurationSnapshot
    ↓
Cosmos alarm-configuration
    PK /partition_key
    ↓
qualification
    ↓
Materialization READY
    runtime.json
    delivery.json
    manifest.json
    ready.json
```

READY no equivale a EFFECTIVE.

## Runtime CURRENT

Runtime adopta exact artifact y publica:

```text
runtime/state/effective-head.json
runtime/output/current/latest.json
runtime/output/facts/*
```

## Modeler CURRENT

Lee CURRENT + exact materialized configs y publica:

```text
modeler/output/current/index.json
modeler/output/current/tools/<hash>/latest.json
```

El baseline selecciona `ACTIVE + PREDOMINANT + VISIBLE + assigned/targeted`, ordena por `priority_order`, `started_at`, identity y occurrence, y expone hasta seis slots.

No implementa todavía carousel/QIQ.

## Delivery CURRENT

Delivery consume únicamente el current Modeler head y publica a Cosmos.

```text
container fijo: alarm-live-projection
partition key contractual: /tool_key
connection registry: tool_key -> endpoint/database/credential variable names
```

Delivery no consume FACTS ni recalcula Modeler logic.

## Live Projection CURRENT

Documento:

```text
ada_alarm_projection_snapshot v1
```

Incluye:

```text
alarms
operator_pool
operator_view
meta
artifact_ref
snapshot_timestamp
tool_key
sha256
```

`ranking` no existe; usar `priority_order`.

## Separate projections

```text
Live                  CURRENT baseline
Management            PLANNED
History/Analytics     PLANNED
```

## Resource preparation

El E2E local aprovisionó explícitamente `alarm-configuration` y `alarm-live-projection`. La preparación productiva/startup gate sigue OPEN; no inferir provisión automática en Azure.

## NEXT único

Command Center Web debe consumir `alarm-live-projection` sin leer WAL/CURRENT directo ni recalcular scheduling.
