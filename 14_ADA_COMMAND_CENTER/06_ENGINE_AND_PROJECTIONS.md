# ADA Command Center — Engine and Projections

Estado: **CURRENT ENGINE / B.2 MATERIALIZATION + EFFECTIVE ALIGNMENT REFINED / DELIVERY EXECUTION STILL PLANNED**

## Authority checkpoint

Implementación auditada:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Canonical base de este delta:

```text
moragaga/atlanticus-cannonical:main
ed49507dbfd808585eb0eb9b89ad1f48b8b3f5a5
```

## Alarm Configuration base Projection

Alarm Configuration SourceRelease se materializa como Projection base exacta.

PRE-SAVE validation y Materialization validation son capas distintas.

B.2 vuelve a validar la revisión persistida contra dependencias actuales antes de producir artifacts operacionales.

## Tool Catalog

Command Center dispone de Tool Catalog V1 durable y read model para authoring.

Tool Catalog no forma parte del payload durable de Alarm Configuration y no reemplaza B.2.

B.2 requiere además current reconciliation qualification de las Tools referenciadas; el contrato exacto de ese input continúa OPEN.

## Alarm Engine CURRENT

Runtime implementa:
- priority predominance por `priority_order`;
- Management suppression de lower-priority Rules independiente de `kind`;
- timer reappearance CURRENT mediante due resolver global;
- Special Condition reappearance mediante `PlannedAlarm.reappearance_special_conditions`;
- level-trigger semantics;
- routing continuo durante Management;
- durable Engine WAL/commits;
- hot snapshots por `priority_group`.

La Web no debe reimplementar estas reglas.

## Durable facts vs hot state

```text
Engine cycle
    |
    +--> durable commit facts / WAL
    |
    `--> GroupRuntimeSnapshot hot state
         runtime/state/groups/<priority_group>.json
```

`GroupRuntimeSnapshot` sirve continuidad/recovery; no se congela como API pública de Delivery.

## Runtime Configuration vs evaluator code

La configuración referencia:

```text
family_key + evaluator_key
```

B.2 valida la key pero no serializa callable.

Runtime une:

```text
RuntimeAlarmConfiguration
+ deployed AlarmEvaluatorRegistry
-> AlarmExecutionSession
```

## Una resolución, dos artifacts

```text
B.2 Resolution
    AlarmResolutionKey
        |
        +--> Runtime Configuration Artifact
        `--> Delivery Configuration Artifact
```

Ambos comparten exactamente el mismo key.

```text
READY
-> ambos artifacts existen

BLOCKED
-> ninguno existe
```

## READY vs EFFECTIVE

```text
B.2 READY
-> candidato coherente

Runtime Adoption succeeds
-> AlarmEffectiveConfigurationHead advances
```

`READY != EFFECTIVE`.

Delivery y Management Capture siguen el Effective Head exacto y nunca lideran Runtime.

## Effective Configuration Head

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
AlarmEffectiveConfigurationHead
    resolution_key
    effective_at
    adoption_id
```

Es global, no por priority group.

Hot snapshot provenance y Effective Head tienen responsabilidades distintas. Un snapshot puede conservar una `state_basis` histórica anterior cuando la nueva Adoption no mutó ese grupo.

El Effective Head debe materializarse desde el mismo Runtime WAL/recovery protocol, no como un JSON independiente escrito después sin protección crash-safe.

Ver `04_ALARM_ENGINE/13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md`.

## Exact-key consumption

Delivery usa:

```text
EffectiveHead.resolution_key
        |
        v
DeliveryConfiguration[exact key]
```

Management Capture usa la misma regla.

No usar latest READY ni fallback a otra revision si falta el artifact exacto.

## Visibility

Authoring:

```text
VISIBLE
TRACE_ONLY
```

PROJECT CONTRACT AGREED:

```text
TRACE_ONLY
-> sigue evaluation/trace/lifecycle/routing/priority/management
-> Delivery no lo publica visiblemente
```

No mapear a `PlannedAlarm.delivery_enabled=false`.

Target:

```text
PlannedAlarm.delivery_enabled -> REMOVE
PriorityDisposition.SHADOW -> REMOVE
visibility_mode -> Delivery Configuration only
```

Si la Rule predominante es TRACE_ONLY, Delivery no promueve otra Rule visible eclipsada. Priority sigue siendo autoridad backend.

## Deactivation + Messages

B.2 resuelve:

```text
ResolvedDeactivationPolicy
    enabled
    max_duration_hours
    approval_required
```

por Rule default y por Message activo aplicando override completo.

Management Capture usa la configuración EFFECTIVE exacta y materializa:

```text
DeactivationIntent
    effective_until
    approval_required
```

El Engine no necesita Message, configured max ni shift-end.

Target:

```text
PlannedAlarm.deactivation_policy -> REMOVE
```

El origen concreto de `shift_end` permanece OPEN.

## Management Capture vs Management Projection

No confundir:

```text
Management Capture
-> valida una intención futura contra la config EFFECTIVE
-> produce input operacional para Engine
```

con:

```text
Management Projection
-> read-side histórico derivado de hechos durables ya resueltos
```

Management Capture provenance mínima acordada incluye:

```text
resolution_key
source_occurrence_id
selected_message_key?
```

El outcome `EFFECTIVE / ADDITIONAL / LATE` sigue siendo autoridad del Engine.

## Reappearance target

B.2 materializa:

```text
AlarmDefinition.after_minutes
-> PlannedAlarm.reappearance_after_seconds
```

Target ManagementEffect:

```text
reappearance_due_at: datetime | None
```

El resolver global CURRENT se elimina como target.

Adoption reconcilia timers abiertos desde el `ManagementEffect.effective_at` original.

Special Condition refs se califican en B.2 y continúan usando trigger level-triggered en Runtime.

## Runtime Adoption

Adoption debe clasificar:

```text
source.defined_alarm_identities
UNION
target.defined_alarm_identities
```

Target dispositions:

```text
UNCHANGED
COMPATIBLE
ADDED
ENABLED
DISABLED
REMOVED
STRUCTURAL_RESET
REJECTED
```

Una Adoption puede avanzar EFFECTIVE con cero group-state commits, por ejemplo ante cambios sólo de visibility, Messages o visual metadata.

## Future Engine → Delivery operational boundary

Delivery no debe leer el WAL como API operacional ni recalcular priority.

Frontera target:

```text
Engine resolved current state
+ Delivery Configuration[EffectiveHead.resolution_key]
        |
        v
Delivery
        |
        v
Alarm Live Projection
```

El schema concreto de `Engine resolved current state` sigue OPEN y es parte del siguiente foco.

## Management vs Live

Managed/deactivated no significa physical false.

Live Projection expresa estado operacional actual derivado.

Management Projection expresa acciones/decisiones históricas.

No mezclar ambas superficies.

## History / Analytics

History/Analytics consume hechos durables y no modifica Engine.

El boundary Engine → History/Analytics → Web permanece separado del foco B.2.

## Provenance cleanup

CURRENT usa repetidamente:

```text
alarm_configuration_revision
tool_registry_revision
```

Target, cuando ambos representan la misma base operacional:

```text
AlarmResolutionKey
```

Occurrence conserva `resolution_key_at_start`; Effective Head conserva la resolución global actual.

No crear aliases legacy permanentes.
