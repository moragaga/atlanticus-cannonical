# Alarm Engine — Modeler and Delivery Pipeline

Estado: **HISTORICAL BASELINE IMPLEMENTED en Command Center / ADVANCED SCHEDULER PLANNED / integración desde el nuevo Runtime UNVERIFIED**. Actualizado 2026-10-10.

Checkpoint:

```text
atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856
```

El checkpoint `atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856` identifica la generación **histórica**. El nuevo Runtime publicado en `atlanticus@c3b8ed3b8de4bbafdaeeff4410d4daaa20bed1b4` emite `ada_alarm_engine_durable_current_state` v1 y FACTS stream/cursor v4; estos contratos **no sustituyen automáticamente** el `Runtime CURRENT v1` esperado por el Modeler aquí descrito. Mantener esta frontera OPEN hasta qualification downstream.

## 1. HISTORICAL pipeline

```text
Runtime CURRENT v1
    + exact EFFECTIVE/READY
        ↓
alarms-modeler
        ↓
current/index.json
current/tools/<tool-hash>/latest.json
        ↓
alarms-delivery
        ↓
Cosmos alarm-live-projection
```

## 2. Modeler CURRENT responsibilities

Modeler:

```text
validates EFFECTIVE/current exact pin
reopens exact materialized runtime + delivery config
filters authoritative eligible alarms
orders candidates
builds operator_pool
builds first-six operator_view
persists current per-Tool snapshots atomically
validates existing index/snapshot checksums on recovery
```

Modeler no ejecuta evaluators ni priority.

## 3. Eligibility CURRENT

```text
ACTIVE
PREDOMINANT
materialized active
VISIBLE
assigned to Tool
visual_target includes Tool
```

## 4. Ordering CURRENT

```text
priority_order
started_at
alarm_identity
occurrence_id
```

`operator_pool` conserva todos los elegibles ordenados.

`operator_view` selecciona hasta `MAX_VISIBLE_SLOTS = 6` y asigna slots 1..6.

## 5. Snapshot contract CURRENT

Per Tool:

```text
id
document_type = ada_alarm_projection_snapshot
schema_version = 1
artifact_ref
snapshot_timestamp
tool_key
alarms
operator_pool
operator_view
meta
sha256
```

Index:

```text
document_type = ada_alarm_modeler_projection_index
schema_version = 1
artifact_ref
snapshot_timestamp
snapshots[{tool_key,path,sha256}]
sha256
```

## 6. Recovery CURRENT

Modeler recovery valida un index existente y sus snapshots.

Si la fuente tiene el mismo `snapshot_timestamp` y el mismo contenido, retorna `CURRENT_UNCHANGED`.

Si aparece una fuente más antigua, retorna `STALE_SOURCE`.

No existe todavía `ModelerState` separado para timers/colas.

## 7. Delivery CURRENT

Delivery consume el index current, valida checksums/exact pin y crea una `CosmosDispatchTask` por Tool.

`ParallelCosmosPublisher` usa concurrencia acotada.

Contrato:

```text
ALARM_PROJECTION_CONTAINER_NAME = alarm-live-projection
```

Connection registry:

```text
connections[tool_key]
    endpoint_var
    database_var
    credential_var
```

La configuración física de container no varía por Tool.

## 8. Delivery observability CURRENT

Una publicación exitosa marca iteration work y reporta:

```text
alarm_delivery_current_status=CURRENT_AVAILABLE
alarm_delivery_published_documents=N
alarm_delivery_failed_tools=0
```

## 9. CAROUSEL — DESIGN FROZEN parcial / NOT IMPLEMENTED

Se conserva la decisión de seis posiciones y el tratamiento especial de `DISTRIBUTED` cuando existan 2+ elegibles.

El baseline first-six no implementa ese scheduler.

Duración `90 vs 120 s`: OPEN.

## 10. QUEUE_IN_QUEUE — DESIGN FROZEN parcial / NOT IMPLEMENTED

Topología congelada:

```text
MINE  4 components / 3 visible
PLANT 5 components / 3 visible
max visible 6
```

Fairness exacta: OPEN.

## 11. Advanced Modeler state — PLANNED

Cuando se implemente scheduling real, Modeler deberá poseer estado suficiente para:

```text
queues
rotation timers
logical slots
reconciliation
durable checkpoint/state
restart without replaying missed visual frames
```

No afirmar que este estado ya existe.

## 12. Runtime → Modeler evolution — OPEN

CURRENT head es suficiente para el baseline actual.

Si scheduling futuro requiere cada transición, definir un handoff durable/ordered/no-drop explícito con checkpoint del Modeler.

## 13. Modeler → Delivery semantics

CURRENT usa latest current head. Delivery puede republicar idempotentemente el snapshot vigente.

Per-destination durable checkpoints/retry state no están implementados todavía.

## 14. Frozen invariants

```text
Runtime is operational truth.
Modeler does not recompute priority.
Delivery does not model.
Web must not schedule.
All stages use the same exact artifact.
TRACE_ONLY is not visible.
ranking does not exist; use priority_order.
container is alarm-live-projection.
```
