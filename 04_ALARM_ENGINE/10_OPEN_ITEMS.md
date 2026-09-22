# Alarm Engine — Open Items

Estado: **OPEN / B.2 DESIGN LARGELY CLOSED / DOMAIN EXTRACTION CLOSED / MATERIALIZATION CONTRACTS NEXT**

Cerrado antes de este checkpoint:
- persistence/recovery qualification;
- concurrency leases/fencing;
- Management suppression por `priority_order`;
- Special Condition Runtime reappearance;
- Special Condition level-trigger semantics.

Cerrado en este hito:
- Command Center Alarm Domain Extraction;
- `scopes/ada-command-center/domain/alarms` como autoridad transversal;
- root replacement sin aliases legacy;
- Engine Core consumiendo Domain;
- Runtime declarando Domain cuando usa sus contratos;
- Web Alarm Configuration consumiendo Domain sin depender de Engine Core por authoring DTOs;
- Configuration Manager consumiendo Domain explícitamente;
- tests de definición/configuración movidos al owner de dominio.

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
- Management round-trip sin evidence devuelto como autoridad;
- B.2 pure resolver/package boundary;
- B.2 process orchestration boundary.

## OPEN relevantes

1. **B.2 materialization contracts**
   - target `backend/alarms/materialization`;
   - implementar value objects/DTOs ya acordados;
   - sin I/O ni resolver completo en el primer incremento.

2. **Pure B.2 resolver**
   - implementar después de los contratos;
   - resolution/validation contra inputs explícitos;
   - no mezclar con process orchestration.

3. **Materialization process**
   - target `backend/processes/alarms-materialization`;
   - acquisition/orchestration/persistence posterior al pure resolver;
   - artifact stores aún OPEN.

4. **Owner/package de Live Delivery**
   - todavía no implementado;
   - debe consumir `EngineResolvedCurrentState` + exact `DeliveryAlarmConfiguration`;
   - no recalcular Engine semantics.

5. **Current Tool reconciliation qualification**
   - Confirmed Tool Catalog + reconciliation-GREEN actual;
   - contrato exacto de este segundo input aún no implementado/verificado.

6. **Cause/evidence contract**
   - falta schema evaluator suficientemente explícito para validar placeholders de `cause_template` antes del Runtime.

7. **Management Capture implementation details**
   - `shift_end` provider;
   - persistence de Captured Management Input;
   - stale/unavailable outcomes;
   - pending request stale cleanup.

8. **Runtime provenance cleanup**
   - reemplazar pares históricos por `AlarmResolutionKey` donde representen una sola base;
   - `resolution_key_at_start` para occurrence provenance;
   - no aliases/adapters permanentes.

9. **Adoption implementation gaps**
   - C1/C3 routing mutation;
   - evaluator/kind/priority-group/origin Tool transitions;
   - timer/Special Condition reconciliation target.

10. **Adoption persistence**
    - journal record discriminado y schema/version;
    - `adoption_id` generation;
    - materialización Effective Head;
    - migration desde persistence CURRENT;
    - crash/recovery tests.

11. **Tool routing qualification adicional**
    - PROCESS ↔ INTEGRATED_OPERATIONS constraints;
    - routing tier matrix;
    - Rule area vs Tool scope cuando corresponda.

12. **Operation details**
    - cadence/event triggers;
    - persistence física READY/BLOCKED y Live Projection;
    - schema versions/codecs;
    - retention;
    - deployment/container topology.

## Conflictos/OPEN que no deben resolverse silenciosamente

B.1 frozen Special Cascade sigue distinto de la suppression uniforme CURRENT por ranking.

B.1 contiene texto que exige Message activo durante B.2, mientras el Project acordó:

```text
inactive Message = válido pero no seleccionable para nuevas gestiones
```

La reconciliación formal en `atlanticus-decisions` sigue pendiente.

Project baseline:

```text
Python 3.14.7
```

Packages Command Center CURRENT auditados:

```text
requires-python ==3.14.2
```

El conflicto permanece OPEN y fuera de este incremento.

## Foco siguiente único

```text
B.2 — Materialization Contracts
```

Implementar sólo contratos puros en:

```text
scopes/ada-command-center/backend/alarms/materialization
```

No abrir aún resolver completo, I/O, stores, job orchestration, Runtime Adoption ni Live Delivery.
