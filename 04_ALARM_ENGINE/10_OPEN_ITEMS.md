# Alarm Engine — Open Items

Estado: **OPEN / B.2 DESIGN LARGELY CLOSED / DOMAIN EXTRACTION PREREQUISITE NEXT**

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
- Management round-trip sin evidence devuelto como autoridad;
- B.2 pure resolver/package boundary;
- B.2 process orchestration boundary.

## Prerequisito de implementación

Antes de implementar B.2 funcional:

```text
Command Center — Alarm Domain Extraction
```

Target:

```text
scopes/ada-command-center/domain/alarms
```

Debe concentrar fundamentos + authoring definitions + `AlarmConfiguration` aggregate/document contract compartidos por Web y Backend.

Ver `../14_ADA_COMMAND_CENTER/17_DOMAIN_OWNERSHIP_AND_MIGRATION.md`.

## OPEN relevantes

1. **Domain extraction implementation**
   - root replacement sin aliases;
   - actualización coordinada de Web + Engine imports;
   - tests de dominio movidos a su nuevo owner;
   - comportamiento sin cambios.

2. **B.2 materialization implementation**
   - target pure capability `backend/alarms/materialization`;
   - target process `backend/processes/alarms-materialization`;
   - artifact stores aún OPEN.

3. **Owner/package de Live Delivery**
   - construir/consumir `EngineResolvedCurrentState`;
   - exact-key join con `DeliveryAlarmConfiguration`;
   - materializar `cause_text`;
   - publicar snapshot sin recalcular Engine semantics.

4. **Current Tool reconciliation qualification**
   - Confirmed Tool Catalog + reconciliation-GREEN actual;
   - contrato exacto de este segundo input aún no implementado/verificado.

5. **Cause/evidence contract**
   - falta schema evaluator suficientemente explícito para validar placeholders de `cause_template` antes del Runtime.

6. **Management Capture implementation details**
   - `shift_end` provider;
   - persistence de Captured Management Input;
   - stale/unavailable outcomes;
   - pending request stale cleanup.

7. **Runtime provenance cleanup**
   - reemplazar pares históricos por `AlarmResolutionKey` donde representen una sola base;
   - `resolution_key_at_start` para occurrence provenance;
   - no aliases/adapters permanentes.

8. **Adoption implementation gaps**
   - C1/C3 routing mutation;
   - evaluator/kind/priority-group/origin Tool transitions;
   - timer/Special Condition reconciliation target.

9. **Adoption persistence**
   - journal record discriminado y schema/version;
   - `adoption_id` generation;
   - materialización Effective Head;
   - migration desde persistence CURRENT;
   - crash/recovery tests.

10. **Tool routing qualification adicional**
    - PROCESS ↔ INTEGRATED_OPERATIONS constraints;
    - routing tier matrix;
    - Rule area vs Tool scope cuando corresponda.

11. **Operation details**
    - cadence/event triggers;
    - persistence física READY/BLOCKED y Live Projection;
    - schema versions/codecs;
    - retention;
    - deployment/container topology.

## Conflictos/OPEN que no deben resolverse silenciosamente

B.1 frozen Special Cascade sigue distinto de la suppression uniforme CURRENT por ranking.

B.1 contiene texto que exige Message activo durante B.2, mientras el Project acordó `inactive Message = válido pero no seleccionable para nuevas gestiones`. La reconciliación formal en `atlanticus-decisions` sigue pendiente.

Project baseline Python 3.14.7 difiere de `requires-python ==3.14.2` observado en packages CURRENT de Command Center. No mezclar ese cambio con Domain extraction salvo bloqueo real.

## Foco siguiente único

```text
Command Center — Alarm Domain Extraction
```

Mover ownership sin cambiar semántica; B.2 funcional comienza en un incremento posterior.
