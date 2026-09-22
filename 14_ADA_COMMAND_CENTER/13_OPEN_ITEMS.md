# ADA Command Center — Open Items

Estado: **OPEN / B.2 IN PROGRESS / MATERIALIZATION + ADOPTION FOUNDATION CONTRACT LARGELY CLOSED**

Cerrado antes de este checkpoint:
- Tool Catalog V1;
- Alarm Tool References V1;
- structured Alarm Configuration authoring;
- Management suppression por `priority_order`;
- Special Condition Runtime reappearance;
- level-trigger semantics de Special Condition.

Cerrado en diseño B.2 y todavía **NOT YET IMPLEMENTED**:
- `AlarmResolutionKey`;
- `READY | BLOCKED`;
- findings `BLOCKING | WARNING`;
- atomicidad Runtime + Delivery;
- `INVALID != REMOVED`;
- Runtime artifact serializable sin evaluator code;
- disabled Rules definidas pero no ejecutables;
- evaluator qualification por key;
- C1/C2/C3 routing materialization;
- C2 cumulative waits;
- Strategic Tool no elegible para Alarm Configuration;
- Deactivation + Messages policy resolution;
- Management Capture aligned al exact Effective key;
- `DeactivationIntent(effective_until, approval_required)` target;
- reappearance timer materialization;
- Special Condition qualification;
- TRACE_ONLY sólo como Delivery visibility;
- `AlarmEffectiveConfigurationHead`;
- durable global Adoption incluso sin hot-state mutations;
- `ADDED` y `ENABLED` adoption dispositions.

## Foco único CURRENT

```text
B.2 — Delivery Configuration Artifact + Live Projection Boundary
IN PROGRESS
```

## B.2 OPEN

### 1. Delivery Configuration schema

Debe compartir `resolution_key` con Runtime Configuration y ser suficiente para Live Delivery y Management Capture sin volver a SharePoint/Tool Catalog/MessageDefinition.

Cerrar:
- resolved display/title/cause contract;
- classification/color/areas;
- visibility;
- resolved Messages;
- default + per-Message deactivation capability;
- resolved visual targets;
- Process projection mode;
- schema version.

### 2. Engine resolved current-state output

CURRENT existe hot runtime state interno.

OPEN:
- contrato explícito Engine resolved current state → Delivery;
- no usar WAL como API de Live Delivery;
- no recalcular priority en Delivery/Web;
- estado dinámico de management/deactivation/assignment requerido por Live.

### 3. Materialization owner

Definir owner/package backend concreto sin acoplar Runtime a:
- SharePoint download;
- Tool discovery;
- Message resolution;
- cross-configuration validation.

### 4. Current Tool reconciliation input

B.2 necesita:

```text
Confirmed Tool Catalog
+
current reconciliation-GREEN qualification
```

El segundo contrato aún no está implementado/verificado.

### 5. Management Capture implementation

OPEN:
- proveedor concreto de `shift_end`;
- storage físico de Captured Management Input;
- stale/unavailable submission outcomes.

### 6. Runtime provenance cleanup

Reemplazar, donde corresponda:

```text
alarm_configuration_revision + tool_registry_revision
```

por:

```text
AlarmResolutionKey
```

sin aliases permanentes.

### 7. Adoption implementation gaps

Mantener visibles:
- origin Tool;
- evaluator key;
- kind;
- priority group;
- C1 routing mutation;
- C3 routing mutation;
- timer/SC reconciliation aún no implementada.

B.2 READY no implica que Runtime CURRENT pueda adoptar toda transición.

### 8. Adoption persistence

OPEN de implementación:
- journal record discriminado;
- `adoption_id` generation;
- Effective Head materialization;
- migration del persistence existente;
- crash/recovery tests del batch global.

### 9. Tool routing qualification adicional

OPEN:
- PROCESS ↔ INTEGRATED_OPERATIONS constraints;
- routing tier matrix;
- Rule area vs Tool scope cuando corresponda.

### 10. Operation details

OPEN:
- Materialization cadence/event trigger;
- persistence física de artifacts/findings;
- retention/versioning;
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

Canonical mantiene el conflicto visible hasta que `atlanticus-decisions` registre explícitamente el refinamiento.
