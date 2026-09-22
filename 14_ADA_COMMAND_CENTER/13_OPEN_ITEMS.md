# ADA Command Center — Open Items

Estado: **OPEN / B.2 IN PROGRESS / FOUNDATION CONTRACT PARTIALLY CLOSED**

Cerrado antes de este checkpoint:
- Tool Catalog V1;
- Alarm Tool References V1;
- structured Alarm Configuration authoring;
- Management suppression por `priority_order`;
- Special Condition Runtime reappearance;
- level-trigger semantics de Special Condition.

Cerrado en diseño durante B.2 y todavía **NOT YET IMPLEMENTED**:
- resolution identity/provenance mínima;
- `READY | BLOCKED`;
- findings `BLOCKING | WARNING`;
- atomicidad Runtime + Delivery;
- `INVALID != REMOVED`;
- Runtime artifact serializable separado de evaluator code;
- disabled Rules continúan definidas y deben seguir siendo válidas;
- evaluator qualification por key;
- C1/C2/C3 routing materialization;
- C2 cumulative waits absolutos;
- Strategic Tool no elegible como Alarm Configuration reference.

## Foco único CURRENT

```text
B.2 — Alarm Configuration -> Runtime/Delivery Configuration Materialization
IN PROGRESS
```

Siguiente subfoco acordado:

```text
Deactivation + Messages materialization
```

## B.2 OPEN

### 1. Materialization owner

Definir owner/package backend concreto sin acoplar Runtime a:
- SharePoint download;
- Tool discovery;
- Message resolution;
- cross-configuration validation.

### 2. Current Tool reconciliation input

B.2 necesita:

```text
Confirmed Tool Catalog
+
current reconciliation-GREEN qualification
```

El segundo contrato todavía no está implementado/verificado.

### 3. Visibility

OPEN:

```text
visibility_mode=TRACE_ONLY
!=
PlannedAlarm.delivery_enabled=false
```

Resolver sin alterar priority/Management involuntariamente.

### 4. Deactivation + Messages

OPEN:
- Rule default;
- Message override completo;
- enabled/disabled capability;
- configured max duration;
- approval requirement;
- operator-selected until;
- shift-end/effective_until;
- distribución entre Runtime, Management input y Delivery Configuration.

### 5. Delivery Configuration schema

Debe compartir `resolution_key` con Runtime Configuration y ser suficiente para Live Delivery sin volver a SharePoint/Tool Catalog/unresolved catalogs.

Schema detallado todavía PLANNED.

### 6. Runtime provenance cleanup

CURRENT:

```text
tool_registry_revision
```

B.2:

```text
confirmed_tool_catalog_revision
```

Implementar reemplazo limpio, sin aliases permanentes.

### 7. Adoption conflicts

Mantener visibles:
- origin Tool;
- evaluator key;
- kind;
- priority group;
- C1 routing mutation;
- C3 routing mutation.

B.2 READY no implica que Runtime Adoption pueda adoptar toda transición hoy.

### 8. Tool routing qualification adicional

OPEN:
- PROCESS ↔ INTEGRATED_OPERATIONS constraints;
- routing tier matrix;
- Rule area vs Tool scope cuando corresponda.

No inventar sin decisión explícita.

### 9. Engine current-state output hacia Delivery

CURRENT existe hot runtime state interno.

OPEN:
- contrato explícito Engine resolved current state → Delivery;
- no usar WAL como API de Live Delivery;
- no hacer que Delivery recalcule priority.

### 10. Operation details

OPEN:
- Materialization cadence/event trigger;
- persistence física de artifacts/findings;
- retention/versioning;
- exact schema versions;
- final deployment/container topology.

## Fuera del foco inmediato

- UI final;
- Message editor final;
- visual presentation terminology;
- application shell final;
- History/Analytics;
- Live/Analytics cadence;
- Golden Path end-to-end posterior a los contratos backend.

## Decisions conflict

B.1 frozen Special Cascade todavía difiere de la implementación CURRENT.

Canonical debe mostrar el conflicto hasta que `atlanticus-decisions` registre explícitamente el refinamiento.

## Cross-cutting

Cualquier conflicto de baseline Python existente fuera de este milestone no debe corregirse silenciosamente dentro de B.2.
