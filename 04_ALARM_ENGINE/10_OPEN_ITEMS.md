# Alarm Engine — Open Items

Estado: **OPEN / B.2 FOCUSED / MATERIALIZATION + ADOPTION + LIVE DELIVERY CONTRACT CLOSED IN DESIGN**

Cerrado antes de este checkpoint:
- persistence/recovery qualification;
- concurrency leases/fencing;
- Management suppression por `priority_order`;
- Special Condition Runtime reappearance;
- Special Condition level-trigger semantics.

Cerrado en diseño B.2 y todavía **NOT YET IMPLEMENTED**:
- `AlarmResolutionKey` y `READY | BLOCKED`;
- atomicidad Runtime + Delivery;
- Runtime artifact serializable y `DISABLED != REMOVED`;
- evaluator qualification por key;
- C1/C2/C3 routing materialization;
- Deactivation + Messages policy resolution;
- Management Capture aligned al exact Effective key;
- `DeactivationIntent(effective_until, approval_required)` target;
- Special Condition qualification;
- `reappearance_after_seconds` y nullable Runtime due;
- TRACE_ONLY exclusivamente como Delivery visibility;
- eliminación target de `delivery_enabled`/`SHADOW`;
- `AlarmEffectiveConfigurationHead`;
- same-WAL `ConfigurationAdoptionCommit`;
- Adoption sobre union de defined identities con `ADDED`/`ENABLED`;
- `DeliveryAlarmConfiguration` shape;
- `EngineResolvedCurrentState`;
- backend materialization de `cause_text`;
- Live publication rules;
- Management round-trip sin evidence devuelto como autoridad.

## OPEN relevantes al último tramo B.2

1. **Owner/package concreto de Configuration Resolution / Materialization**
   - package y responsibility boundary;
   - dependencias permitidas;
   - artifact stores;
   - no acoplar Engine a SharePoint/Tool discovery/Message resolution.

2. **Owner/package de Live Delivery**
   - construir `EngineResolvedCurrentState`;
   - join exacto con `DeliveryAlarmConfiguration`;
   - materializar `cause_text`;
   - publicar snapshot lógico sin recalcular Engine semantics.

3. **Current Tool reconciliation qualification**
   - B.2 requiere Confirmed Tool Catalog + reconciliation-GREEN actual;
   - el contrato exacto de este segundo input aún no está implementado/verificado.

4. **Cause/evidence contract**
   - CURRENT `EvidenceSnapshot` aporta `contract_key`, `contract_version`, `payload`;
   - falta un schema por evaluator suficientemente explícito para validar placeholders de `cause_template` antes del Runtime.

5. **Management Capture implementation details**
   - proveedor concreto de `shift_end`;
   - persistence física de `CapturedManagementInput`;
   - stale/unavailable submission outcomes;
   - cleanup/invalidation autónoma de pending requests stale.

6. **Runtime provenance cleanup**
   - reemplazar pares históricos por `AlarmResolutionKey` donde representen una sola base;
   - `resolution_key_at_start` para occurrence provenance;
   - no aliases/adapters permanentes.

7. **Adoption implementation gaps**
   - C1/C3 routing mutation;
   - evaluator/kind/priority-group/origin Tool transitions;
   - timer/Special Condition reconciliation target.

8. **Adoption persistence**
   - journal record discriminado y schema/version;
   - `adoption_id` generation;
   - materialización del Effective Head;
   - migration desde persistence CURRENT;
   - crash/recovery tests.

9. **Tool routing qualification adicional**
   - PROCESS ↔ INTEGRATED_OPERATIONS constraints;
   - routing tier matrix;
   - Rule area vs Tool scope cuando corresponda.

10. **Operation details**
    - cadence/event triggers;
    - persistence física de READY/BLOCKED y Live Projection;
    - schema versions/codecs;
    - retention;
    - deployment/container topology.

## Conflictos/OPEN que no deben resolverse silenciosamente

B.1 frozen Special Cascade sigue distinto de la suppression uniforme CURRENT por ranking.

Además, B.1 contiene texto que exige Message activo durante B.2, mientras el Project acordó `inactive Message = válido pero no seleccionable para nuevas gestiones`. La reconciliación formal en `atlanticus-decisions` sigue pendiente.

## Foco siguiente único

```text
B.2 — Materialization Owner/Package + Implementation Boundary
```

Este foco debe convertir los contratos ya cerrados en un incremento backend-first pequeño y verificable, sin mezclar UI final, Analytics/History ni broad Engine rewrite.
