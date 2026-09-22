# Alarm Engine — Open Items

Estado: **OPEN / B.2 FOCUSED / MATERIALIZATION + ADOPTION FOUNDATION CONTRACT LARGELY CLOSED IN DESIGN**

Cerrado antes de este checkpoint:
- persistence/recovery qualification;
- concurrency leases/fencing;
- Management suppression por `priority_order`;
- Special Condition Runtime reappearance;
- Special Condition level-trigger semantics.

Cerrado en diseño B.2 y todavía **NOT YET IMPLEMENTED**:
- `AlarmResolutionKey` = Alarm Configuration revision + Confirmed Tool Catalog revision;
- `AlarmConfigurationResolution` con `READY | BLOCKED`;
- findings `BLOCKING | WARNING`;
- atomicidad Runtime + Delivery;
- `INVALID != REMOVED`;
- Runtime artifact serializable separado del evaluator callable;
- `defined_alarm_identities` vs executable `PlannedAlarm`;
- validation de Rules/steps disabled;
- evaluator qualification por key;
- C1/C2/C3 routing materialization;
- C2 cumulative waits;
- Strategic no elegible como Alarm Configuration Tool reference;
- Deactivation + Messages policy resolution;
- Management Capture provenance por `resolution_key + occurrence + message`;
- `DeactivationIntent(effective_until, approval_required)` target;
- eliminación target de `PlannedAlarm.deactivation_policy`;
- Special Condition qualification completa;
- `reappearance_after_seconds` materialization;
- `ManagementEffect.reappearance_due_at: datetime | None` target;
- eliminación target de `ReappearanceDueAtResolver` global;
- TRACE_ONLY exclusivamente como visibility Delivery;
- eliminación target de `PlannedAlarm.delivery_enabled` y `PriorityDisposition.SHADOW`;
- `AlarmEffectiveConfigurationHead` global;
- `ConfigurationAdoptionCommit` durable;
- same-WAL adoption + recovery semantics;
- Adoption sobre union de defined identities;
- dispositions `ADDED` y `ENABLED`.

## OPEN relevantes a B.2

1. **Delivery Configuration artifact schema**
   - contenido exacto por Rule;
   - resolved Messages/deactivation capability;
   - resolved visual targets;
   - visibility;
   - schema version.

2. **Engine resolved current-state output hacia Delivery**
   - no usar WAL como API operacional;
   - no hacer que Delivery recalcule priority;
   - definir payload dinámico suficiente para Live Projection.

3. **Owner/package concreto de Configuration Resolution / Materialization**
   - frontera backend y dependencias;
   - no acoplar Engine a loaders de SharePoint/Cosmos.

4. **Current Tool reconciliation qualification**
   - B.2 requiere Tool en Confirmed Tool Catalog **y** reconciliation-GREEN actual;
   - contrato exacto todavía no implementado/verificado.

5. **Management Capture implementation details**
   - proveedor concreto de `shift_end`;
   - persistence física de `CapturedManagementInput`;
   - exact outcome contract para stale/unavailable submissions.

6. **Runtime provenance implementation cleanup**
   - reemplazar `alarm_configuration_revision + tool_registry_revision` por `AlarmResolutionKey` donde ambos representen una única base;
   - `resolution_key_at_start` para occurrence provenance;
   - no aliases/adapters permanentes.

7. **Adoption implementation gaps**
   - C1 routing mutation CURRENT = rejected;
   - C3 routing mutation CURRENT = rejected;
   - evaluator mutation CURRENT = rejected;
   - kind mutation CURRENT = rejected;
   - priority group mutation CURRENT = rejected;
   - origin Tool semantics conflict;
   - timer/Special Condition reconciliation todavía no implementada.

8. **Adoption persistence**
   - journal record discriminado;
   - schema/version exacta;
   - `adoption_id` generation;
   - materialization del Effective Head;
   - migration desde persistence CURRENT.

9. **Tool routing qualification adicional**
   - PROCESS ↔ INTEGRATED_OPERATIONS constraints;
   - routing tier matrix;
   - Rule area vs Tool scope cuando corresponda.

10. **Materialization operation details**
    - cadence/event trigger;
    - persistence física de READY/BLOCKED diagnostics/artifacts;
    - retention/versioning;
    - final deployment/container topology.

## Conflicto de decisiones todavía visible

B.1 frozen Special Cascade no coincide con la implementación CURRENT de suppression uniforme por ranking.

```text
IMPLEMENTATION CURRENT / TESTED
PROJECT REFINEMENT AGREED
DECISIONS UPDATE PENDING
```

No declarar formalmente superseded hasta registrarlo en `atlanticus-decisions`.

## Foco siguiente único

```text
B.2 — Delivery Configuration Artifact + Live Projection Boundary
```

No mezclar con UI final, Analytics/History ni broad Engine rewrite.
