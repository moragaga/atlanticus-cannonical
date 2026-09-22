# Alarm Engine — Open Items

Estado: **OPEN / PURE B.2 RESOLVER IMPLEMENTED / MATERIALIZATION PROCESS NEXT**

Checkpoint:

```text
moragaga/atlanticus:main
9398786ae9af7c00de1bcca9d7a311fe9ef2155f
```

## CLOSED / CURRENT

- Command Center Alarm Domain Extraction.
- Root replacement de autoridades authored legacy.
- Management suppression por `priority_order`.
- Special Condition Runtime reappearance.
- deactivation como fuente independiente de lower-rank cascade suppression.
- deactivation domina causal attribution si Management y deactivation coexisten.
- `PlannedAlarm.delivery_enabled` removido.
- `PriorityDisposition.SHADOW` removido.
- `AlarmResolutionKey` implementado en Alarm Core.
- `PlannedAlarm.reappearance_after_seconds` implementado.
- package `backend/alarms/materialization`.
- `AlarmConfigurationResolution`.
- Runtime/Delivery materialization contract types.
- atomicidad `READY | BLOCKED`.
- `ToolReconciliationQualification`.
- `EvaluatorQualificationKey`.
- `EvaluatorQualificationCatalog`.
- pure deterministic B.2 resolver implementado y exportado.
- C1/C2/C3 materialization implementada.
- active-message selection y full deactivation override implementados.
- visual target qualification/materialization implementada.
- reappearance minutes -> seconds implementada.

## Qualification pendiente mínima

La última evidencia compartida confirma:
- 31 tests PASS;
- Ruff lint PASS.

No conserva una salida posterior explícita del formatter tras la corrección/commit.

Estado:

```text
final ruff format --check evidence
UNVERIFIED
```

Gate de entrada del siguiente incremento: confirmarlo una vez. No es un nuevo frente de arquitectura.

## OPEN relevantes

### 1. Materialization Process — NEXT

Target:

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

Debe diseñarse antes de implementar.

Responsabilidad:
- acquisition;
- revision comparison cuando corresponda;
- resolver invocation;
- artifact/findings persistence;
- diagnostics;
- retry/exit/operational behavior.

No meter esta responsabilidad dentro del resolver.

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

### 4. Artifact persistence

OPEN:
- Runtime Configuration store;
- Delivery Configuration store;
- findings/history;
- codecs/schema versions;
- retention;
- exact physical keys/containers.

### 5. Runtime provenance cleanup

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

### 6. Reappearance reconciliation during Adoption

Runtime shape y B.2 materialization ya están CLOSED/CURRENT.

Sigue OPEN reconciliar hot state cuando una revisión EFFECTIVE cambia:
- timer;
- special condition references.

No reabrir Special Condition Runtime semantics ya implementadas.

### 7. Deactivation Core ownership cleanup

`PlannedAlarm.deactivation_policy` sigue CURRENT.

El comportamiento de deactivation cascade ya está CLOSED y no depende de este cleanup.

### 8. Cause/evidence contract

Falta schema evaluator explícito para validar placeholders de `cause_template` antes de Runtime.

El resolver CURRENT no inventa esa validación.

### 9. Runtime Adoption / Effective Head

OPEN:
- discriminated journal record;
- `adoption_id`;
- Effective Head materialization;
- migration;
- crash/recovery tests;
- ADDED/ENABLED transitions;
- integración con artifact stores;
- reconciliation de reappearance.

### 10. Live Delivery owner/package

Contrato de diseño cerrado; implementación física pendiente.

### 11. Management Capture

OPEN:
- `shift_end` provider;
- persistence;
- stale/unavailable outcomes;
- pending deactivation cleanup.

### 12. Tool routing qualification adicional

OPEN deliberadamente fuera del resolver actual:
- PROCESS ↔ INTEGRATED_OPERATIONS constraints;
- routing tier matrix;
- Rule area vs Tool scope.

No agregarlos retroactivamente sin contrato.

### 13. Operation details

OPEN:
- materialization/Live cadence;
- event trigger;
- physical containers/partitions;
- final deployment topology;
- degree of automatic vs controlled manual operation.

### 14. Python baseline conflict

```text
Project baseline: Python 3.14.7
ADA Command Center backend: requires-python ==3.14.2
```

La qualification compartida del resolver ejecutó CPython 3.14.2.

No mezclar este conflicto con el Materialization Process salvo bloqueo demostrado.

## Historical conflicts visibles

B.1 Special Cascade sigue distinto de suppression CURRENT por ranking.

B.1 no expresa la deactivation vigente como fuente independiente de cascade suppression.

B.1 exige Message activo en una formulación histórica; CURRENT considera inactive Message válido
pero no seleccionable.

B.1 no define Alarm projection Strategic; CURRENT resolver bloquea Strategic visual targets.

El detalle C2 relativo -> acumulado absoluto queda refinado por implementación CURRENT.

`atlanticus-decisions` no está reconciliado.

## Foco siguiente único

```text
B.2 MATERIALIZATION PROCESS
```

Primera acción:
- verificar formatter GREEN sobre `atlanticus:main` CURRENT;
- si GREEN, continuar inmediatamente con diseño del process.

No abrir Runtime Adoption, Live Delivery, Management Capture, deactivation ownership cleanup ni broad
Core cleanup en el mismo incremento.
