# ADA Command Center — Open Items

Estado: **OPEN / ALARM DOMAIN EXTRACTION CLOSED / B.2 MATERIALIZATION CONTRACTS NEXT**

Cerrado antes de este checkpoint:
- Tool Catalog V1;
- Alarm Tool References V1;
- structured Alarm Configuration authoring;
- Management suppression por `priority_order`;
- Special Condition Runtime reappearance;
- B.2 design through Delivery/Live boundary.

Cerrado en este checkpoint:
- Alarm Configuration como dominio transversal de Command Center;
- `scopes/ada-command-center/domain/alarms`;
- `ada_command_center.domain.alarms`;
- shared fundamentals `AlarmIdentity`, `AlarmKind`, `Criticality`;
- authoring definitions bajo Domain;
- `AlarmConfiguration` aggregate/validation/document contract bajo Domain;
- Web Configuration y Backend Alarm Core consumiendo un único authored contract;
- root replacement sin aliases legacy;
- tests de dominio bajo su owner correcto;
- eliminación de las autoridades antiguas `core/definition.py` y Web `models.py`.

## Foco único CURRENT

```text
B.2 — Materialization Contracts
PLANNED / NEXT IMPLEMENTATION INCREMENT
```

Target:

```text
scopes/ada-command-center/backend/alarms/materialization
```

## OPEN

### 1. B.2 materialization contracts

Implementar sólo los contratos puros ya acordados:
- `AlarmResolutionKey`;
- `AlarmResolutionFinding`;
- `AlarmConfigurationResolution`;
- `RuntimeAlarmConfiguration`;
- `DeliveryAlarmConfiguration`;
- `ResolvedDeliveryAlarm`;
- `ResolvedDeliveryMessage`;
- `ResolvedDeactivationPolicy`;
- resolved visual target value objects;
- enums/value objects estrictamente necesarios por esos contratos.

No implementar en el mismo incremento:
- resolver B.2 completo;
- acquisition;
- stores;
- scheduler;
- job/process orchestration;
- Runtime Adoption;
- Live Delivery;
- Management Capture.

### 2. Pure B.2 resolver

PLANNED después de contracts.

Debe resolver/validar contra inputs explícitos y producir `READY | BLOCKED` sin I/O físico.

### 3. B.2 materialization process

PLANNED después del resolver.

Target:

```text
backend/processes/alarms-materialization
```

Responsabilidad:
- acquisition;
- revision comparison;
- resolver invocation;
- artifact/findings persistence;
- process diagnostics.

### 4. Live Delivery owner/package

El contrato está cerrado en diseño; owner físico todavía no está implementado.

### 5. Current Tool reconciliation input

B.2 necesita:

```text
Confirmed Tool Catalog
+
current reconciliation-GREEN qualification
```

El segundo contrato todavía no está implementado/verificado.

### 6. Cause/evidence schema

OPEN:
- schema contractual para validar placeholders de `cause_template` contra evaluator output antes del Runtime;
- diagnostics físicos exactos de `MATERIALIZATION_ERROR`.

### 7. Management Capture implementation

OPEN:
- proveedor concreto de `shift_end`;
- storage físico de Captured Management Input;
- stale/unavailable submission outcomes;
- cleanup/invalidation autónoma de pending deactivation requests stale.

### 8. Runtime provenance cleanup

Reemplazar donde corresponda:

```text
alarm_configuration_revision + tool_registry_revision
```

por `AlarmResolutionKey`, sin aliases permanentes.

### 9. Adoption implementation gaps

Mantener visibles:
- origin Tool;
- evaluator key;
- kind;
- priority group;
- C1/C3 routing mutation;
- timer/SC reconciliation.

### 10. Adoption persistence

OPEN:
- journal record discriminado;
- `adoption_id`;
- Effective Head materialization;
- migration existente;
- crash/recovery tests.

### 11. Tool routing qualification adicional

OPEN:
- PROCESS ↔ INTEGRATED_OPERATIONS constraints;
- routing tier matrix;
- Rule area vs Tool scope.

### 12. Operation details

OPEN:
- materialization/Live cadence;
- physical stores/containers/partitions;
- schema versions/codecs;
- retention;
- final deployment topology.

### 13. Python baseline conflict

VERIFIED conflict:

```text
Project baseline: Python 3.14.7
CURRENT Command Center packages: requires-python ==3.14.2
```

No mezclarlo con B.2 Materialization Contracts salvo bloqueo real.

## Fuera del foco inmediato

- UI final;
- History/Analytics;
- Live/Analytics cadence tuning;
- final visual presentation;
- broad Engine rewrite.

## Decisions conflicts visibles

B.1 frozen Special Cascade todavía difiere de la implementation CURRENT de suppression por ranking.

B.1 contiene una formulación que exige Message activo en B.2; el Project refinó esto a:

```text
inactive Message = válido pero no seleccionable para nuevas gestiones
```

La reconciliación formal en `atlanticus-decisions` sigue pendiente.
