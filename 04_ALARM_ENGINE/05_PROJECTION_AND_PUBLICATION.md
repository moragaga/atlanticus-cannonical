# Alarm Engine — Projection and Publication

Estado: **CURRENT — RUNTIME CURRENT/FACTS + MODELER LIVE SNAPSHOT + DELIVERY COSMOS IMPLEMENTED**

## 1. Runtime publications

Runtime publica:

```text
runtime/output/current/latest.json
runtime/output/facts/facts-<hash>.json
runtime/output/state/facts-export-cursor.json
```

### CURRENT v1

```text
document_type = ada_command_center_engine_resolved_current_state
schema_version = 1
artifact_ref
state.resolution_key
state.as_of
state.alarms[]
sha256
```

Alarm current contiene, entre otros:

```text
identity
occurrence_id
episode_id
started_at
evaluation
priority
assignments
pending_assignments
technical_hold
management/deactivation fields
```

### FACTS v2

Durable commit facts permanecen canal separado. El live Modeler baseline no necesita FACTS para construir el snapshot actual.

## 2. Modeler input CURRENT

Modeler exige:

```text
EFFECTIVE head
Runtime CURRENT v1
exact READY runtime.json
delivery.json del mismo exact artifact
```

Si CURRENT no corresponde al EFFECTIVE exacto, espera y no modela.

## 3. Modeler projection CURRENT

Index:

```text
modeler/output/current/index.json

document_type = ada_alarm_modeler_projection_index
schema_version = 1
artifact_ref
snapshot_timestamp
snapshots[] { tool_key, path, sha256 }
sha256
```

Snapshot por Tool:

```text
modeler/output/current/tools/<sha256(tool_key)>/latest.json

document_type = ada_alarm_projection_snapshot
schema_version = 1
id
artifact_ref
snapshot_timestamp
tool_key
alarms
operator_pool
operator_view
meta
sha256
```

## 4. Eligibility CURRENT

Se proyecta sólo cuando:

```text
evaluation.status == ACTIVE
priority.disposition == PREDOMINANT
materialized alarm is_active
visibility_mode == VISIBLE
Runtime assignment incluye tool_key
visual_target incluye tool_key
```

## 5. Ordering/current view

Orden:

```text
priority_order
started_at
alarm_identity
occurrence_id
```

`operator_pool` contiene todos los candidatos elegibles de ese Tool ordenados.

`operator_view` contiene como máximo 6, asignados a slots 1..6.

No hay `ranking` separado.

## 6. Delivery CURRENT

Delivery consume exclusivamente el Modeler current head.

Valida index/snapshots/digests/exact pin y publica cada Tool mediante `ParallelCosmosPublisher`.

Contrato físico:

```text
container = alarm-live-projection
partition key = /tool_key
```

Connection registry:

```text
config/connections.json
connections[tool_key] = {
    endpoint_var,
    database_var,
    credential_var,
}
```

El container no varía por Tool.

## 7. Web boundary

Web debe leer el snapshot modelado. No debe leer WAL, reabrir READY, recalcular priority ni reconstruir `operator_view`.

## 8. Separate projections

Permanecen separadas:

```text
Live Projection       CURRENT baseline
Management Projection PLANNED
History/Analytics     PLANNED
```
