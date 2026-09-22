# Alarm Engine — Open Items

Estado: **OPEN / B.2 CONTRACTS CLOSED / QUALIFICATION INPUT CONTRACTS CLOSED / PURE RESOLVER NEXT**

Checkpoint:

```text
moragaga/atlanticus:main
bc3fffd72afb712d5b5ab84522c379abf2a19642
```

## CLOSED / CURRENT

- Command Center Alarm Domain Extraction.
- Root replacement de autoridades authored legacy.
- Management suppression por `priority_order`.
- Special Condition Runtime reappearance.
- `PlannedAlarm.delivery_enabled` removido.
- `PriorityDisposition.SHADOW` removido.
- `AlarmResolutionKey` implementado en Alarm Core.
- package `backend/alarms/materialization`.
- `AlarmConfigurationResolution`.
- Runtime/Delivery materialization contract types.
- atomicidad `READY | BLOCKED`.
- `ToolReconciliationQualification`.
- `EvaluatorQualificationKey`.
- `EvaluatorQualificationCatalog`.

## OPEN relevantes

### 1. Pure B.2 resolver — NEXT

Implementar resolución/validación determinística contra inputs explícitos.

No I/O.

No process orchestration.

Debe producir solamente `AlarmConfigurationResolution`.

### 2. Tool reconciliation qualification producer

El contrato de consumo existe:

```text
ToolReconciliationQualification(green_tool_keys)
```

Sigue OPEN quién/qué lo construye a partir del estado actual de reconciliation.

No inventar taxonomía RED/DRIFT/MISSING dentro de Materialization.

### 3. Evaluator qualification producer

El contrato de consumo existe:

```text
EvaluatorQualificationCatalog
```

Sigue OPEN cómo se deriva desde el deployed evaluator registry/catalog sin transportar callables ni
`DataRequirement` al artifact.

### 4. Materialization process

Target:

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

Responsabilidad futura:
- acquisition;
- revision comparison;
- resolver invocation;
- artifact/findings persistence;
- process diagnostics.

### 5. Artifact persistence

OPEN:
- Runtime Configuration store;
- Delivery Configuration store;
- findings/history;
- codecs/schema versions;
- retention.

### 6. Runtime provenance cleanup

CURRENT todavía usa pares históricos donde corresponde:

```text
alarm_configuration_revision
tool_registry_revision
```

TARGET:

```text
AlarmResolutionKey
resolution_key_at_start
```

Sin aliases permanentes.

### 7. Reappearance timer target

OPEN:
- `reappearance_after_seconds`;
- reconciliation en Adoption;
- nullable Runtime due.

No reabrir Special Condition Runtime semantics ya implementadas.

### 8. Deactivation Core cleanup

`PlannedAlarm.deactivation_policy` sigue CURRENT.

Target Delivery/Management Capture permanece acordado, pero cleanup no se hizo en este hito.

### 9. Cause/evidence contract

Falta schema evaluator explícito para validar placeholders de `cause_template` antes de Runtime.

### 10. Runtime Adoption / Effective Head

OPEN:
- discriminated journal record;
- `adoption_id`;
- Effective Head materialization;
- migration;
- crash/recovery tests;
- ADDED/ENABLED transitions;
- integration con artifact stores.

### 11. Live Delivery owner/package

Contrato de diseño cerrado; implementación física pendiente.

### 12. Management Capture

OPEN:
- `shift_end` provider;
- persistence;
- stale/unavailable outcomes;
- pending deactivation cleanup.

### 13. Tool routing qualification adicional

OPEN:
- PROCESS ↔ INTEGRATED_OPERATIONS constraints;
- routing tier matrix;
- Rule area vs Tool scope.

### 14. Operation details

OPEN:
- materialization/Live cadence;
- event trigger;
- physical containers/partitions;
- final deployment topology.

### 15. Python baseline conflict

```text
Project baseline: Python 3.14.7
Command Center packages: requires-python ==3.14.2
```

No mezclar con pure resolver salvo bloqueo demostrado.

## Historical conflicts visibles

B.1 Special Cascade sigue distinto de suppression CURRENT por ranking.

B.1 exige Message activo en una formulación histórica; Project CURRENT considera:

```text
inactive Message = válido pero no seleccionable para nuevas gestiones
```

`atlanticus-decisions` no está reconciliado.

## Foco siguiente único

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
```

No abrir materialization process, Adoption, Live Delivery, Management Capture ni broad Core cleanup
en el mismo incremento.
