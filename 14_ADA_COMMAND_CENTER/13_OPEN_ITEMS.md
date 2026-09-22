# ADA Command Center — Open Items

Estado: **OPEN / B.2 IN PROGRESS / DELIVERY + LIVE BOUNDARY CLOSED IN DESIGN**

Cerrado antes de este checkpoint:
- Tool Catalog V1;
- Alarm Tool References V1;
- structured Alarm Configuration authoring;
- Management suppression por `priority_order`;
- Special Condition Runtime reappearance.

Cerrado en diseño B.2 y todavía **NOT YET IMPLEMENTED**:
- Resolution `READY | BLOCKED`;
- Runtime + Delivery atomic artifacts;
- Deactivation/Messages policy resolution;
- reappearance materialization;
- TRACE_ONLY sólo como Delivery visibility;
- Effective Configuration Head y durable Adoption;
- `DeliveryAlarmConfiguration`;
- `EngineResolvedCurrentState`;
- backend `cause_text` materialization desde current evidence;
- exact-key Live join;
- publication de `PREDOMINANT | DEACTIVATED` visibles;
- exclusión de `ECLIPSED`, `CASCADE_SUPPRESSED` y TRACE_ONLY;
- current pending deactivation request en Live state;
- Management round-trip basado en identity/occurrence/resolution, no evidence devuelto por Web.

## Foco único CURRENT

```text
B.2 — Materialization Owner/Package + Implementation Boundary
IN PROGRESS
```

## B.2 OPEN

### 1. Materialization owner/package

Definir el package backend concreto que:
- adquiere Published Alarm Configuration + Confirmed Tool Catalog + reconciliation qualification;
- ejecuta B.2;
- persiste READY/BLOCKED findings y artifacts;
- no convierte Runtime en downloader/resolver externo.

### 2. Live Delivery owner/package

Definir el package que:
- recibe `EngineResolvedCurrentState`;
- carga `DeliveryAlarmConfiguration` del exact Effective key;
- materializa `cause_text`;
- publica el snapshot Live completo;
- no recalcula lifecycle, priority, routing ni Message precedence.

### 3. Current Tool reconciliation input

B.2 necesita:

```text
Confirmed Tool Catalog
+
current reconciliation-GREEN qualification
```

El segundo contrato todavía no está implementado/verificado.

### 4. Cause/evidence schema

OPEN:
- schema contractual que permita validar placeholders de `cause_template` contra el output del evaluator antes del Runtime;
- comportamiento físico exacto de diagnostics de `MATERIALIZATION_ERROR`.

La regla ya cerrada es que un error de cause no oculta una occurrence operacional real.

### 5. Management Capture implementation

OPEN:
- proveedor concreto de `shift_end`;
- storage físico de Captured Management Input;
- stale/unavailable submission outcomes;
- cleanup/invalidation autónoma de pending deactivation requests stale.

### 6. Runtime provenance cleanup

Reemplazar donde corresponda:

```text
alarm_configuration_revision + tool_registry_revision
```

por `AlarmResolutionKey`, sin aliases permanentes.

### 7. Adoption implementation gaps

Mantener visibles:
- origin Tool;
- evaluator key;
- kind;
- priority group;
- C1/C3 routing mutation;
- timer/SC reconciliation.

### 8. Adoption persistence

OPEN:
- journal record discriminado;
- `adoption_id`;
- Effective Head materialization;
- migration existente;
- crash/recovery tests.

### 9. Tool routing qualification adicional

OPEN:
- PROCESS ↔ INTEGRATED_OPERATIONS constraints;
- routing tier matrix;
- Rule area vs Tool scope.

### 10. Operation details

OPEN:
- materialization/Live cadence;
- physical stores/containers/partitions;
- schema versions/codecs;
- retention;
- final deployment topology.

## Fuera del foco inmediato

- UI final;
- History/Analytics;
- Live/Analytics cadence tuning;
- final visual presentation;
- broad Engine rewrite.

## Decisions conflicts visibles

B.1 frozen Special Cascade todavía difiere de la implementation CURRENT de suppression por ranking.

B.1 también contiene una formulación que exige Message activo en B.2; el Project refinó esto a `inactive Message = válido pero no seleccionable para nuevas gestiones`. La reconciliación formal en `atlanticus-decisions` sigue pendiente.
