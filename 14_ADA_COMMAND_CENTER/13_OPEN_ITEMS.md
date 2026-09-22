# ADA Command Center — Open Items

Estado: **OPEN / B.2 CONTRACTS CLOSED / QUALIFICATION INPUT CONTRACTS CLOSED / PURE RESOLVER NEXT**

Checkpoint:

```text
moragaga/atlanticus@bc3fffd72afb712d5b5ab84522c379abf2a19642
```

## CLOSED

- Alarm Domain Extraction.
- authored root replacement sin aliases legacy.
- Tool Catalog V1.
- Alarm Tool References V1.
- structured Alarm Configuration authoring.
- Management suppression por `priority_order`.
- Special Condition Runtime reappearance.
- `delivery_enabled` removal.
- `SHADOW` removal.
- `AlarmResolutionKey` implementation.
- `backend/alarms/materialization` contract package.
- Runtime/Delivery resolution artifacts.
- READY/BLOCKED atomic resolution contract.
- Tool reconciliation qualification input contract.
- evaluator qualification input contract.

## Foco único CURRENT

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
PLANNED / NEXT
```

## OPEN

### 1. Pure B.2 resolver

Debe consumir inputs explícitos y producir `AlarmConfigurationResolution`.

No I/O físico.

No partial artifacts.

### 2. Current Tool reconciliation producer

El consumer contract ya existe:

```text
ToolReconciliationQualification(green_tool_keys)
```

Falta productor/adquisición concreta del estado GREEN.

### 3. Evaluator qualification producer

El consumer contract ya existe:

```text
EvaluatorQualificationCatalog
```

Falta integración concreta con deployed evaluator registry/catalog.

### 4. B.2 materialization process

Target:

```text
backend/processes/alarms-materialization
```

Después del resolver:
- acquisition;
- revision comparison;
- invocation;
- persistence;
- diagnostics.

### 5. Runtime/Delivery artifact stores

OPEN:
- physical store;
- exact-key loading;
- codecs/schema versions;
- retention.

### 6. Runtime provenance cleanup

Migrar pares históricos hacia `AlarmResolutionKey` donde corresponda.

`resolution_key_at_start` para occurrence provenance.

Sin aliases permanentes.

### 7. Reappearance timer target

`reappearance_after_seconds` y Adoption reconciliation siguen OPEN.

### 8. Deactivation Core cleanup

`PlannedAlarm.deactivation_policy` sigue CURRENT.

Target Management Capture/Delivery permanece pendiente de implementación.

### 9. Cause/evidence schema

Falta schema evaluator para validar placeholders de `cause_template`.

### 10. Runtime Adoption / Effective Head

No implementado.

### 11. Live Delivery

Owner físico no implementado.

### 12. Management Capture

Proveedor `shift_end`, persistence y stale handling pendientes.

### 13. Tool routing qualification adicional

PROCESS/INTEGRATED_OPERATIONS, tiers y area/scope pendientes.

### 14. Operation details

Cadence, triggers, physical topology y retention pendientes.

### 15. Python baseline

```text
3.14.7 vs requires-python ==3.14.2
```

OPEN.

## Decisions conflicts

B.1 Special Cascade vs suppression CURRENT por ranking.

Historical active-Message requirement vs:

```text
inactive Message = valid but non-selectable
```

No resolver silenciosamente.

## Fuera del foco inmediato

- UI final;
- History/Analytics;
- Live cadence;
- broad Engine rewrite.
