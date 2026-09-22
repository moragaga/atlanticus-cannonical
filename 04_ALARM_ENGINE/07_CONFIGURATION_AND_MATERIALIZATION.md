# Alarm Engine — Configuration and Materialization

Estado: **B.2 CONTRACTS IMPLEMENTED / QUALIFICATION INPUT CONTRACTS IMPLEMENTED / PURE RESOLVER PLANNED NEXT / ADOPTION + LIVE DELIVERY NOT IMPLEMENTED**

## Authority checkpoint

Implementación auditada:

```text
moragaga/atlanticus:main
bc3fffd72afb712d5b5ab84522c379abf2a19642
```

Canonical base de este cierre:

```text
moragaga/atlanticus-cannonical:main
2d8cbc33b7776e057e4f7d82def318d5eaf8f336
```

Decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Invariantes centrales

```text
LATEST SAVED = LATEST VALID_AT_SAVE

VALID_AT_SAVE != READY != EFFECTIVE

INVALID != REMOVED
DISABLED != INVALID
DISABLED != REMOVED
TRACE_ONLY != REMOVED
READY != EFFECTIVE
```

B.2 no elimina silenciosamente una Rule rota para construir un artifact parcial.

## Owner CURRENT

Implementado:

```text
scopes/ada-command-center/backend/alarms/materialization
```

Distribución:

```text
ada-command-center-alarms-materialization==1.0.0
```

Namespace:

```python
ada_command_center.alarms.materialization
```

Dependencias CURRENT:
- Alarm Core;
- Alarm Domain;
- `ada-web-tools` para `ToolConfigurationKind`.

No contiene resolver completo, I/O, stores, scheduler ni orchestration.

## AlarmResolutionKey CURRENT

Owner físico:

```text
ada_command_center.alarms.core.AlarmResolutionKey
```

Shape:

```text
alarm_configuration_revision
confirmed_tool_catalog_revision
```

No contiene evaluator revision.

## Resolution contracts CURRENT

```text
AlarmResolutionStatus
    READY
    BLOCKED

AlarmResolutionFindingSeverity
    BLOCKING
    WARNING

AlarmResolutionFinding
    code
    severity
    message
    alarm_identity?
    field_path?
    reference_key?

AlarmConfigurationResolution
    resolution_key
    status
    findings
    runtime_configuration?
    delivery_configuration?
```

Atomicidad implementada:

```text
READY
<=> no BLOCKING
<=> Runtime existe
<=> Delivery existe
<=> Runtime.key == Delivery.key == resolution key

BLOCKED
<=> al menos un BLOCKING
<=> Runtime is None
<=> Delivery is None
```

No existe readiness parcial.

## Runtime Configuration CURRENT

```text
RuntimeAlarmConfiguration
    resolution_key
    defined_alarm_identities
    planned_alarms
    parameters_by_alarm
```

Invariantes implementadas:
- no identities definidas duplicadas;
- no plans duplicados;
- todo planned alarm debe estar definido;
- las revisions CURRENT del `PlannedAlarm` deben coincidir con `AlarmResolutionKey`;
- parameters sólo para planned alarms;
- valores de parámetros: `str | float | bool`.

Representación:

```text
active executable
-> defined + PlannedAlarm

disabled
-> defined + no PlannedAlarm

removed
-> absent
```

No contiene evaluator callable, `DataRequirement`, `DataLoadPlan`, `AlarmExecutionSession`,
Message catalog ni visibility metadata.

## Delivery Configuration CURRENT

```text
DeliveryAlarmConfiguration
    resolution_key
    alarms: tuple[ResolvedDeliveryAlarm, ...]
```

`ResolvedDeliveryAlarm` CURRENT:

```text
identity
is_active
visibility_mode
display_name
title
cause_template
kind
criticality
business_category
operational_areas
color
default_deactivation_policy
messages
visual_targets
```

`ResolvedDeliveryMessage`:

```text
message_key
display_text
deactivation_policy
```

`ResolvedDeactivationPolicy`:

```text
enabled
max_duration_hours
approval_required
```

Visual target contracts:

```text
ResolvedVisualTarget
    tool_key
    tool_kind
    component_keys
    subcomponents
    process_projection_mode?

ResolvedVisualSubcomponentTarget
    owner_component_key
    subcomponent_key
```

Delivery admite Rules disabled y TRACE_ONLY; removed significa ausencia.

No contiene evaluator, parameters, priority source data, routing, reappearance, lifecycle hot state
ni `ToolStructure`.

## Qualification input contracts CURRENT

Implementado:

```text
ToolReconciliationQualification
    green_tool_keys: tuple[str, ...]
    is_green(tool_key) -> bool
```

Invariantes:
- tuple;
- keys no vacías;
- sin duplicados;
- orden determinístico.

Un Tool no listado no se etiqueta RED/MISSING/DRIFT: solamente no está GREEN para B.2.

Implementado:

```text
EvaluatorQualificationKey
    family_key
    evaluator_key

EvaluatorQualificationCatalog
    qualified_keys
    is_qualified(family_key, evaluator_key) -> bool
```

Invariantes:
- pair no vacío;
- sin duplicados;
- orden determinístico;
- sin callable;
- sin `DataRequirement`;
- sin Runtime registry.

La producción/adquisición concreta de ambos inputs sigue fuera del package contractual.

## Pure B.2 resolver — NEXT / NOT IMPLEMENTED

Frontera objetivo:

```text
AlarmConfiguration
+ alarm_configuration_revision
+ Confirmed Tool Catalog
+ ToolReconciliationQualification
+ EvaluatorQualificationCatalog
        |
        v
pure deterministic resolver
        |
        v
AlarmConfigurationResolution
```

Acquisition pertenece al futuro Materialization Process.

El resolver no debe leer Cosmos, Blob, SharePoint ni Runtime stores.

## Validations acordadas que el resolver deberá cubrir

Cuando se implemente, debe respetar los contratos ya congelados:
- full candidate, no partial Rule publication;
- evaluator qualification para toda Rule definida, incluso disabled;
- Tool references contra exact Confirmed Tool Catalog + GREEN qualification;
- C1/C2/C3 routing materialization;
- Message reference validation y active-message selection;
- deactivation override = full replacement;
- Special Condition reference qualification;
- visual target resolution;
- priority/cross-rule invariants que pertenezcan a B.2;
- all findings determinísticos y sin silent correction.

## Runtime gaps que NO fueron resueltos en este hito

OPEN separados:
- provenance histórica de `PlannedAlarm`/occurrence -> `AlarmResolutionKey`;
- target `reappearance_after_seconds`;
- cleanup de `PlannedAlarm.deactivation_policy`;
- Runtime Adoption/Effective Head.

El pure resolver no debe convertir esos gaps en adapters o contratos paralelos.

## Runtime Adoption / EFFECTIVE

Sigue NOT IMPLEMENTED.

```text
B.2 READY != EFFECTIVE
```

Ver `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md`.

## Live Delivery

Contrato de diseño permanece acordado y no fue implementado en este hito.

Exact-key alignment sigue congelado:

```text
EngineResolvedCurrentState.resolution_key
==
DeliveryAlarmConfiguration.resolution_key
==
AlarmEffectiveConfigurationHead.resolution_key
```

No se modifica `16_ALARM_LIVE_DELIVERY_CONTRACT.md` en este cierre.

## OPEN después de este checkpoint

- pure B.2 resolver;
- productor/adquisición de current Tool reconciliation GREEN;
- productor/adaptación de evaluator qualification;
- `backend/processes/alarms-materialization`;
- artifact stores;
- cause/evidence schema;
- Runtime provenance cleanup;
- reappearance timer target;
- deactivation Core cleanup;
- Runtime Adoption + Effective Head;
- Live Delivery implementation;
- Management Capture;
- routing tier/kind constraints adicionales;
- cadence, codecs, schema versions, retention y deployment topology.

## No reabrir por conveniencia

B.2 no debe:
- reimplementar priority;
- reimplementar Management suppression;
- reimplementar Special Condition Runtime reappearance;
- introducir UI semantics en Core;
- introducir aliases/adapters legacy.
