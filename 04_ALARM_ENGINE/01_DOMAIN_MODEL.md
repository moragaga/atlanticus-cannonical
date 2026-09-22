# Alarm Engine — Domain Model

Estado: **CURRENT / CORE VISIBILITY CLEANUP IMPLEMENTED / B.2 CONTRACT TYPES IMPLEMENTED / TARGET RUNTIME CLEANUPS OPEN**

Realidad implementada auditada:

```text
moragaga/atlanticus:main
bc3fffd72afb712d5b5ab84522c379abf2a19642
```

Canonical base:

```text
moragaga/atlanticus-cannonical:main
2d8cbc33b7776e057e4f7d82def318d5eaf8f336
```

Decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Ownership

Authored Alarm domain:

```text
scopes/ada-command-center/domain/alarms
```

Runtime Engine:

```text
scopes/ada-command-center/backend/alarms/core
```

Pure B.2 materialization contracts:

```text
scopes/ada-command-center/backend/alarms/materialization
```

Core no conoce Cosmos, SharePoint, Dash/Flask, Tool Catalog acquisition, Message catalog resolution
ni editable Alarm Configuration.

## AlarmIdentity

```text
AlarmIdentity(family_key, alarm_key)
```

No introducir `rule_key` paralelo.

Family y `priority_group` son conceptos distintos.

## AlarmResolutionKey CURRENT

Implementado en Alarm Core:

```text
AlarmResolutionKey
    alarm_configuration_revision
    confirmed_tool_catalog_revision
```

Es VO operacional compartido.

No agregar `evaluator_registry_revision` sin contrato real.

Ownership en Core evita la dependencia circular futura:

```text
Core -> Materialization -> Core
```

que ocurriría si el key perteneciera físicamente a Materialization y Core necesitara usarlo para
provenance/adoption.

## PlannedAlarm CURRENT

CURRENT ya no contiene visibility:

```text
identity
kind
criticality
priority_group
priority_order
evaluator_key
alarm_configuration_revision
tool_registry_revision
routing
deactivation_policy
reappearance_special_conditions
```

Removido:

```text
delivery_enabled
```

`PriorityDisposition` CURRENT:

```text
PREDOMINANT
ECLIPSED
CASCADE_SUPPRESSED
DEACTIVATED
```

Removido:

```text
SHADOW
```

Por tanto:

```text
TRACE_ONLY != Runtime flag
TRACE_ONLY != SHADOW
```

## Visibility CURRENT

Authored:

```text
VISIBLE
TRACE_ONLY
```

Runtime:
- evalúa normalmente;
- lifecycle normal;
- routing normal;
- priority normal;
- management/suppression normal;
- no filtra por visibility.

Delivery:
- conserva `visibility_mode`;
- decide publicación visible después de priority Runtime.

Una Rule TRACE_ONLY predominante no promueve una visible eclipsada.

## Priority CURRENT

```text
priority_order > 0
priority_order único dentro del priority_group
menor priority_order = mayor prioridad
```

Management suppression CURRENT se gobierna por `priority_order` e ignora visibility.

## RuntimeAlarmConfiguration CURRENT

Implementado en Materialization:

```text
RuntimeAlarmConfiguration
    resolution_key
    defined_alarm_identities
    planned_alarms
    parameters_by_alarm
```

Invariantes:
- active executable: definida + `PlannedAlarm`;
- disabled: definida sin `PlannedAlarm`;
- removed: ausente;
- plans únicos;
- parameters sólo para planned alarms;
- revisions históricas CURRENT del `PlannedAlarm` deben coincidir con `resolution_key`.

No contiene evaluator callable, `DataRequirement`, `DataLoadPlan` ni `AlarmExecutionSession`.

## Runtime provenance cleanup OPEN

Aunque `AlarmResolutionKey` ya existe, `PlannedAlarm` y occurrence provenance CURRENT todavía usan
pares históricos:

```text
alarm_configuration_revision
tool_registry_revision
```

TARGET:

```text
resolution_key
resolution_key_at_start
```

No implementar aliases permanentes ni doble provenance.

## Deactivation boundary OPEN

`PlannedAlarm.deactivation_policy` todavía existe en Core CURRENT.

El target acordado sigue siendo resolver deactivation contextualmente en Delivery/Management Capture
y no convertir `max_duration_hours`/Message semantics en lifecycle Core.

No mezclar este cleanup con el pure B.2 resolver sin una decisión explícita si se vuelve prerequisite.

## Reappearance boundary OPEN

CURRENT conserva `reappearance_special_conditions`.

El target de `reappearance_after_seconds` y la reconciliación de timer durante Runtime Adoption
siguen abiertos.

Special Condition Runtime reappearance ya está implementada y no se reabre por conveniencia.

## Conflict con B.1 historical

B.1 Special Cascade difiere de suppression CURRENT por ranking.

Estado:

```text
IMPLEMENTATION CURRENT / VALIDATED
PROJECT REFINEMENT AGREED
DECISIONS REPOSITORY NOT RECONCILED
```

No resolver silenciosamente.
