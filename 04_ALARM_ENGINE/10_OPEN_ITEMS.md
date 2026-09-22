# Alarm Engine — Open Items

Estado: **OPEN / B.2 FOCUSED / FOUNDATION CONTRACT PARTIALLY CLOSED**

Cerrado antes de este documento:
- persistence/recovery qualification;
- concurrency leases/fencing;
- Management suppression por `priority_order`;
- Special Condition Runtime reappearance;
- Special Condition level-trigger semantics.

Cerrado en diseño B.2 durante este hito, todavía **NOT YET IMPLEMENTED**:
- `resolution_key` mínimo = Alarm Configuration revision + Confirmed Tool Catalog revision;
- `AlarmConfigurationResolution` con `READY | BLOCKED`;
- findings `BLOCKING | WARNING`;
- atomicidad: BLOCKED no publica Runtime ni Delivery artifacts;
- `INVALID != REMOVED`;
- Runtime artifact serializable separado del evaluator callable;
- `defined_alarm_identities` vs executable `PlannedAlarm` para `DISABLED != REMOVED`;
- validation de Rules/steps disabled;
- B.2 valida evaluator reference pero Runtime vuelve a resolver código desplegado;
- C1/C2/C3 routing materialization;
- C2 cumulative waits absolutos desde occurrence start, incluyendo intervals de steps disabled;
- C3 rechaza enabled destinations;
- Strategic no es elegible como Alarm Configuration Tool reference.

## OPEN relevantes a B.2

1. **Owner/package concreto de Configuration Resolution / Materialization**
   - frontera backend y dependencias;
   - no acoplar Engine a loaders de SharePoint/Cosmos.

2. **Current Tool reconciliation qualification**
   - B.2 requiere Tool en Confirmed Tool Catalog **y** estado reconciliation-GREEN actual;
   - el contrato exacto de ese input todavía no está implementado/verificado.

3. **TRACE_ONLY vs `delivery_enabled`**
   - `TRACE_ONLY` no puede mapearse ciegamente a `delivery_enabled=false`;
   - preservar evaluation/trace/priority/Management y filtrar visibilidad en Delivery.

4. **Deactivation + Messages materialization**
   - Rule default;
   - Message override completo;
   - `enabled`;
   - configured max duration;
   - `approval_required`;
   - operator-selected until;
   - shift-end/effective capability;
   - distribución correcta entre Runtime, Management input y Delivery.

5. **Delivery Configuration artifact schema**
   - comparte `resolution_key` con Runtime artifact;
   - debe permitir Live Delivery sin re-resolver catálogos externos.

6. **Runtime provenance implementation cleanup**
   - reemplazar semánticamente `tool_registry_revision` por Confirmed Tool Catalog revision;
   - no crear aliases/adapters permanentes.

7. **Adoption reconciliation**
   - C1 routing mutation CURRENT = rejected;
   - C3 routing mutation CURRENT = rejected;
   - evaluator mutation CURRENT = rejected;
   - kind mutation CURRENT = rejected;
   - priority group mutation CURRENT = rejected;
   - origin Tool semantics conflict;
   - no resolver silenciosamente dentro de B.2.

8. **Tool routing qualification adicional**
   - PROCESS ↔ INTEGRATED_OPERATIONS constraints;
   - routing tier matrix;
   - no inventar sin decisión explícita.

9. **Materialization operation details**
   - cadence/event trigger;
   - persistence física de READY/BLOCKED diagnostics/artifacts;
   - retention;
   - exact schema versions.

## Conflicto de decisiones todavía visible

B.1 frozen Special Cascade no coincide con la implementación CURRENT de suppression uniforme por ranking.

Estado:

```text
IMPLEMENTATION CURRENT / TESTED
PROJECT REFINEMENT AGREED
DECISIONS UPDATE PENDING
```

No declarar el conflicto formalmente superseded dentro de canonical hasta registrarlo en `atlanticus-decisions`.

## Fuera del foco inmediato

No mezclar con:
- Delivery execution final;
- UI final;
- Analytics/History;
- broad Engine rewrite;
- presentation terminology.

F-010 continúa como baseline final histórico de qualification.
