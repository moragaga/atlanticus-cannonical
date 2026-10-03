# ADA Command Center — Alarm Live Delivery Contract

Estado: **CURRENT — LIVE PROJECTION IMPLEMENTED AND QUALIFIED LOCALLY**

## Ownership

Runtime produce verdad operacional.

Modeler produce el read model lógico current.

Delivery sólo transporta/publica.

Web será consumidor.

## Current chain

```text
EFFECTIVE exact artifact
+
EngineResolvedCurrentState CURRENT v1
+
RuntimeAlarmConfiguration
+
DeliveryAlarmConfiguration
        ↓
Alarm Modeler
        ↓
AlarmProjectionSnapshot v1 per Tool
        ↓
Alarm Delivery
        ↓
Cosmos alarm-live-projection
```

## Exact alignment invariants

```text
READY != EFFECTIVE
same exact artifact_ref across Runtime/Modeler/Delivery
no fallback to latest READY
checksums validated
```

## Snapshot contract CURRENT

```text
document_type = ada_alarm_projection_snapshot
schema_version = 1
id = alarm_projection_snapshot:<tool-hash-prefix>
artifact_ref
snapshot_timestamp
tool_key
alarms
operator_pool
operator_view
meta
sha256
```

`alarms` se indexa por `occurrence_id`.

`operator_pool` contiene occurrence ids elegibles ordenados.

`operator_view` contiene `{slot, occurrence_id}` con máximo 6 slots en el baseline.

## Eligibility

```text
ACTIVE
PREDOMINANT
VISIBLE
active materialized alarm
assigned to Tool
visual target for Tool
```

## Physical publication contract

```text
container = alarm-live-projection
partition key = /tool_key
```

Connection resolution:

```text
config/connections.json
connections[tool_key].endpoint_var
connections[tool_key].database_var
connections[tool_key].credential_var
```

No configurar container por Tool.

## Web contract

Web no recalcula:

```text
priority
routing
eligibility
operator_pool
operator_view
```

Web renderiza el head modelado.

## OPEN

CURRENT conserva `cause_template` y evidence. La causa dinámica efectiva definitiva permanece OPEN.

Management/History siguen fuera de este contrato.
