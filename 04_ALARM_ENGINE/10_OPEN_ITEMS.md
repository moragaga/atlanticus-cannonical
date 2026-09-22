# Alarm Engine — Open Items

Estado: **OPEN / CORE PREREQUISITES CLOSED / B.2 CONTRACTS CLOSED / PURE RESOLVER NEXT**

Checkpoint:

```text
moragaga/atlanticus:main
cd08bd8d2c25bd89eb39fa15cbda209c8e9be617
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

## OPEN relevantes

### 1. Pure B.2 resolver — NEXT

Implementar resolución/validación determinística contra inputs explícitos.

No I/O.

No process orchestration.

Debe producir solamente `AlarmConfigurationResolution`.

Debe materializar, entre otros contratos:

```text
ReappearanceDefinition.after_minutes
-> PlannedAlarm.reappearance_after_seconds
```

con `None -> None` y `M -> M * 60`.

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

### 7. Reappearance reconciliation during Adoption

El Runtime shape ya está CLOSED:

```text
reappearance_after_seconds
reappearance_special_conditions
```

Sigue OPEN reconciliar hot state cuando una revisión EFFECTIVE cambia:
- timer;
- special condition references.

No reabrir Special Condition Runtime semantics ya implementadas.

### 8. Deactivation Core ownership cleanup

`PlannedAlarm.deactivation_policy` sigue CURRENT.

El comportamiento de deactivation cascade ya está CLOSED y no depende de este cleanup.

Target Delivery/Management Capture permanece acordado para policy/context de decisiones futuras.

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
- integración con artifact stores;
- reconciliation de reappearance.

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
ADA Command Center backend: requires-python ==3.14.2
```

El gate local del cierre resolvió CPython 3.14.2.

No mezclar este conflicto con pure resolver salvo bloqueo demostrado.

## Historical conflicts visibles

B.1 Special Cascade sigue distinto de suppression CURRENT por ranking.

B.1 no expresa la deactivation vigente como fuente independiente de cascade suppression.

B.1 exige Message activo en una formulación histórica; Project CURRENT considera:

```text
inactive Message = válido pero no seleccionable para nuevas gestiones
```

`atlanticus-decisions` no está reconciliado.

## Foco siguiente único

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
```

No abrir materialization process, Adoption, Live Delivery, Management Capture,
deactivation ownership cleanup ni broad Core cleanup en el mismo incremento.
