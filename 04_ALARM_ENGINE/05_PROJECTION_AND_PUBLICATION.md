# Alarm Engine — Projection and Publication

Estado: **CURRENT — nuevo Runtime publica CURRENT durable v1 y FACTS stream/cursor v4; Modeler/Delivery del pipeline histórico no cualificados contra estos contratos**. Baseline: `atlanticus@c3b8ed3b8de4bbafdaeeff4410d4daaa20bed1b4` (2026-10-10).

## Nuevo Alarm Runtime — CURRENT implementado

Las publicaciones del nuevo proceso están bajo la raíz operacional de output de Alarm Engine:

```text
current/durable-latest.json
facts/year=YYYY/month=MM/day=DD/hour=HH/part-NNNN.jsonl
state/facts-export-cursor.json
```

### CURRENT durable v1

```text
document_type = ada_alarm_engine_durable_current_state
schema_version = 1
artifact_ref
journal_position
state.resolution_key
state.groups[]
sha256
```

Es una proyección de snapshots de grupo confirmados. Requiere journal alineado, EFFECTIVE exacto y estado derivado de WAL. No puede retroceder ni redefinir la autoridad durable.

### FACTS stream / cursor v4

```text
record document_type = ada_command_center_engine_committed_facts_stream
cursor document_type = ada_command_center_engine_facts_export_cursor
schema_version = 4
stream = segmentos JSONL horarios, part-NNNN
cursor = state/facts-export-cursor.json
```

El contrato compartido vive en `scopes/ada-contracts/alarms/src/ada/contracts/alarms/facts_stream.py` y sus schemas v4. El exporter consume commits atribuidos durables, preserva orden/hash/posición de journal, ancla artifact y avanza cursor tras append validado con fencing. Errores de integridad bloquean publicación; no se infiere migración automática de cursor/volúmenes anteriores.

La publicación y el recovery se coordinan en `processes/alarm-runtime/src/ada/processes/alarm_runtime/composition.py`. **CURRENT** es estado actual; **FACTS** conserva hechos para consumo secuencial. Ninguno es una nueva fuente de autoridad.

## Pipeline histórico Command Center — HISTORICAL, no equivalente

El pipeline físico histórico usaba:

```text
runtime/output/current/latest.json
    Runtime CURRENT v1: ada_command_center_engine_resolved_current_state
runtime/output/facts/facts-<hash>.json
    FACTS batch v2/v3
    ↓ Modeler (exact EFFECTIVE/READY)
modeler/output/current/index.json
modeler/output/current/tools/<tool-hash>/latest.json
    ↓ Delivery
Cosmos alarm-live-projection, partition key /tool_key
```

Su qualification local histórica incluyó Tool snapshot, Delivery y read-back Cosmos. **No prueba** que el Modeler consuma `ada_alarm_engine_durable_current_state`, ni que Delivery consuma el nuevo FACTS v4. La integración nueva Runtime → Modeler → Delivery permanece **OPEN / UNVERIFIED**, no un cambio implícito de contrato.

Los invariantes históricos de Modeler continúan como frontera funcional (sin atribuirlos al Runtime nuevo): elegibilidad ACTIVE + PREDOMINANT + materialized active + VISIBLE + asignación y target de Tool; orden `priority_order, started_at, alarm_identity, occurrence_id`; `operator_pool` ordenado y `operator_view` hasta seis slots. Scheduler avanzado permanece PLANNED.

## Separación congelada

```text
Live Projection       -- visualización operativa vía Modeler/Delivery
Management Projection -- independiente, PLANNED
History/Analytics     -- independiente, PLANNED
```

Web no lee WAL ni usa el CURRENT durable del Runtime como contrato final de UI. Modeler no recalcula prioridad, Delivery no modela y Analytics no escribe en el Engine. Referencias: `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` y `14_MODELER_AND_DELIVERY_PIPELINE.md`.
