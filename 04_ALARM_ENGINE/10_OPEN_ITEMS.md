# Alarm Engine — Open Items

Estado: **OPEN / B.2 FOCUSED**

Cerrado antes de este documento:
- persistence/recovery qualification;
- concurrency leases/fencing;
- Management suppression por `priority_order`;
- Special Condition Runtime reappearance;
- Special Condition level-trigger semantics.

## OPEN relevantes al siguiente foco

1. **B.2 materialization contract**
   - owner concreto;
   - resolution identity/provenance;
   - findings;
   - Runtime readiness;
   - Delivery readiness;
   - `PlannedAlarm` materialization;
   - `AlarmExecutionEntry`/parameters materialization cuando corresponda.

2. **TRACE_ONLY vs `delivery_enabled`**
   - `TRACE_ONLY` no puede mapearse ciegamente a `delivery_enabled=false`;
   - preservar evaluation/trace/priority semantics y filtrar visibilidad en Delivery.

3. **C2 wait materialization**
   - authoring guarda `wait_minutes_from_previous_step`;
   - Engine necesita `delay_seconds` absoluto desde occurrence start;
   - congelar o rechazar explícitamente la propuesta cumulative antes de implementar.

4. **Adoption reconciliation**
   - B.1 vs Engine CURRENT para origin Tool, evaluator, kind y priority group;
   - no resolver silenciosamente.

5. **Runtime provenance**
   - reconciliar `alarm_configuration_revision` y `tool_registry_revision` históricos con B.2.

6. **Special Condition qualification**
   - B.2 debe mapear sólo referencias válidas a `PlannedAlarm.reappearance_special_conditions`;
   - Engine no valida `is_special_condition`.

7. **Deactivation materialization**
   - Rule default;
   - Message override;
   - configured max duration;
   - operator-selected until;
   - effective capability/effective_until.
   - No modificar Engine sin evidencia de gap.

## Conflicto de decisiones todavía visible

B.1 frozen Special Cascade no coincide con la implementación CURRENT de suppression uniforme por ranking.

Estado:

```text
IMPLEMENTATION CURRENT / TESTED
PROJECT REFINEMENT AGREED
DECISIONS UPDATE PENDING
```

No declarar el conflicto resuelto dentro de canonical hasta registrar el refinamiento en `atlanticus-decisions`.

## Fuera del siguiente incremento

No mezclar con:
- UI final;
- Analytics/History;
- Tool tier matrix;
- Message editor;
- visual presentation terminology;
- broad Engine rewrite.

F-010 continúa como baseline final histórico de qualification.
