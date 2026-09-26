# Alarm Engine — Domain Model

Estado: **CURRENT / CORE PREREQUISITES IMPLEMENTED / B.2 CONTRACT TYPES IMPLEMENTED / TARGET CLEANUPS OPEN**

Realidad implementada auditada:

```text
moragaga/atlanticus:main
cd08bd8d2c25bd89eb39fa15cbda209c8e9be617
```

Canonical base:

```text
moragaga/atlanticus-cannonical:main
56943d94889719544f426322ded4a877245dfaee
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

## PlannedAlarm CURRENT

CURRENT no contiene visibility:

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
reappearance_after_seconds
reappearance_special_conditions
```

`reappearance_after_seconds`:

```text
None
or
int > 0
```

`bool`, cero y negativos son inválidos.

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
- management/deactivation suppression normal;
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

La suppression CURRENT se gobierna por `priority_order` e ignora visibility.

## DeactivationEffect CURRENT

```text
DeactivationEffect
    effect_id
    source_occurrence_id
    effective_from
    effective_until
```

La provenance de la occurrence fuente se conserva aunque la occurrence cierre.

Una deactivation vigente es una barrera operacional independiente de Management:

```text
source -> DEACTIVATED

target activo
AND mismo priority_group
AND target.priority_order > source.priority_order
-> CASCADE_SUPPRESSED
```

`CascadeSuppression` conserva exactamente una causa:

```text
management_effect_id XOR deactivation_effect_id
```

Si ambos efectos están vigentes, la causa atribuida es deactivation.

Pending approval no equivale a effect vigente.

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

## Deactivation ownership cleanup OPEN

`PlannedAlarm.deactivation_policy` todavía existe en Core CURRENT.

El target acordado sigue siendo resolver deactivation contextualmente en Delivery/Management Capture
y no convertir Message semantics en lifecycle Core.

Este cleanup está separado de las semánticas de deactivation ya implementadas y probadas.

## Reappearance CURRENT y OPEN

Authored Domain:

```text
ReappearanceDefinition
    after_minutes: int | None
    special_conditions: tuple[AlarmIdentity, ...]
```

Runtime Core CURRENT:

```text
PlannedAlarm.reappearance_after_seconds: int | None
PlannedAlarm.reappearance_special_conditions: tuple[AlarmIdentity, ...]
```

El pure B.2 resolver debe materializar:

```text
after_minutes is None -> reappearance_after_seconds = None
after_minutes = M     -> reappearance_after_seconds = M * 60
```

La reconciliación de timer/special conditions sobre hot state durante Runtime Adoption sigue OPEN.

Special Condition Runtime reappearance ya está implementada y no se reabre por conveniencia.

## Conflict con B.1 historical

B.1 Special Cascade:

```text
managed predominant Special Condition
-> suppress all other active Rules in group
```

difiere de CURRENT:

```text
Management/deactivation suppression
-> sólo targets activos de prioridad numéricamente menor
   (priority_order mayor)
```

La deactivation como fuente independiente de cascade suppression es un refinamiento posterior no
expresado por B.1.

Estado:

```text
IMPLEMENTATION CURRENT / VALIDATED
PROJECT REFINEMENT CURRENT
DECISIONS REPOSITORY HISTORICAL / NOT RECONCILED
```

No resolver silenciosamente.
