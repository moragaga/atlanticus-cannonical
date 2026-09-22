# ADA Command Center — Open Items

Estado: **OPEN / DOMAIN OWNERSHIP CLOSED IN DESIGN / ALARM DOMAIN EXTRACTION NEXT**

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

Cerrado en diseño en este checkpoint:
- Alarm Configuration es dominio transversal de Command Center, no Web-only ni Backend-only;
- target `scopes/ada-command-center/domain/alarms`;
- Web Configuration y Backend Alarm Core dependen del mismo domain package;
- root replacement sin aliases legacy;
- B.2 pure resolver target `backend/alarms/materialization`;
- B.2 operational job target `backend/processes/alarms-materialization`;
- primer incremento debe ser Domain extraction sin cambio semántico.

Ver `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md`.

## Foco único CURRENT

```text
Command Center — Alarm Domain Extraction
PLANNED / NEXT IMPLEMENTATION INCREMENT
```

## OPEN

### 1. Domain extraction implementation

Implementar el reemplazo de raíz:
- crear `scopes/ada-command-center/domain/alarms`;
- mover fundamentals compartidos (`AlarmIdentity`, `AlarmKind`, `Criticality`);
- mover authoring definitions;
- mover `AlarmConfiguration` aggregate/validation/document contract;
- actualizar imports Web + Engine;
- mover tests de dominio;
- eliminar autoridades anteriores;
- no introducir aliases/adapters permanentes.

El incremento debe preservar comportamiento.

### 2. B.2 materialization implementation

Después del Domain extraction:

```text
backend/alarms/materialization
```

para contratos + resolver determinista, y:

```text
backend/processes/alarms-materialization
```

para acquisition/orchestration/persistence.

No implementar ambos en el mismo incremento que la migración de dominio.

### 3. Live Delivery owner/package

Definir/implementar el package que:
- recibe `EngineResolvedCurrentState`;
- carga `DeliveryAlarmConfiguration` del exact Effective key;
- materializa `cause_text`;
- publica el snapshot Live completo;
- no recalcula lifecycle, priority, routing ni Message precedence.

### 4. Current Tool reconciliation input

B.2 necesita:

```text
Confirmed Tool Catalog
+
current reconciliation-GREEN qualification
```

El segundo contrato todavía no está implementado/verificado.

### 5. Cause/evidence schema

OPEN:
- schema contractual que permita validar placeholders de `cause_template` contra output evaluator antes del Runtime;
- physical diagnostics exactos de `MATERIALIZATION_ERROR`.

Una falla de cause no oculta una occurrence operacional real.

### 6. Management Capture implementation

OPEN:
- proveedor concreto de `shift_end`;
- storage físico de Captured Management Input;
- stale/unavailable submission outcomes;
- cleanup/invalidation autónoma de pending deactivation requests stale.

### 7. Runtime provenance cleanup

Reemplazar donde corresponda:

```text
alarm_configuration_revision + tool_registry_revision
```

por `AlarmResolutionKey`, sin aliases permanentes.

### 8. Adoption implementation gaps

Mantener visibles:
- origin Tool;
- evaluator key;
- kind;
- priority group;
- C1/C3 routing mutation;
- timer/SC reconciliation.

### 9. Adoption persistence

OPEN:
- journal record discriminado;
- `adoption_id`;
- Effective Head materialization;
- migration existente;
- crash/recovery tests.

### 10. Tool routing qualification adicional

OPEN:
- PROCESS ↔ INTEGRATED_OPERATIONS constraints;
- routing tier matrix;
- Rule area vs Tool scope.

### 11. Operation details

OPEN:
- materialization/Live cadence;
- physical stores/containers/partitions;
- schema versions/codecs;
- retention;
- final deployment topology.

### 12. Python baseline conflict

VERIFIED conflict:

```text
Project baseline: Python 3.14.7
CURRENT audited Command Center packages: requires-python ==3.14.2
```

No corregir dentro del Domain extraction salvo bloqueo real. Tratar como incremento separado.

## Fuera del foco inmediato

- UI final;
- History/Analytics;
- Live/Analytics cadence tuning;
- final visual presentation;
- broad Engine rewrite.

## Decisions conflicts visibles

B.1 frozen Special Cascade todavía difiere de la implementation CURRENT de suppression por ranking.

B.1 contiene una formulación que exige Message activo en B.2; el Project refinó esto a `inactive Message = válido pero no seleccionable para nuevas gestiones`. La reconciliación formal en `atlanticus-decisions` sigue pendiente.
