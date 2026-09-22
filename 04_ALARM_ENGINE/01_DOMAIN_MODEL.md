# Alarm Engine — Domain Model

Estado: **CURRENT / IMPLEMENTED IN CORE + B.2 TARGET CONTRACT REFINED / B.1 DECISIONS CONFLICT STILL VISIBLE**

Fuentes de intención:
- `R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md`;
- `R3.6M-006B.2-alarm-projection-boundary-DECISION-RECORDED.md`;
- `R3.6M-006B.2-alarm-projection-and-publication-boundary-DECISION-RECORDED-INCREMENT-2.md`;
- refinamientos explícitos del Project implementados y validados en `atlanticus:main`;
- contratos B.2 acordados en Project y todavía no implementados.

Realidad implementada auditada:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Canonical base de este delta:

```text
moragaga/atlanticus-cannonical:main
ed49507dbfd808585eb0eb9b89ad1f48b8b3f5a5
```

## Ownership

Alarm Engine core no conoce:
- Cosmos;
- SharePoint;
- Key Vault;
- Dash/Flask;
- geometría UI;
- sesiones web;
- Alarm Configuration editable;
- Tool Catalog como source de authoring;
- Message Catalog como catálogo por resolver.

Core recibe contratos Runtime ya resueltos/materializados y hechos operacionales ya capturados.

## Conceptos

- `AlarmDefinition`: definición editable canónica de una Rule.
- `AlarmResolutionKey`: identidad compartida de una resolución operacional.
- `PlannedAlarm`: política Runtime resuelta de una Rule ejecutable.
- `AlarmEvaluatorContract`: lógica Python desplegada + `DataRequirements`, resuelta por `(family_key, evaluator_key)`.
- `AlarmExecutionEntry`: unión Runtime de `PlannedAlarm + evaluator contract + parameters`.
- `AlarmEvaluation`: resultado físico/técnico del ciclo.
- `Occurrence`: activación de una Rule.
- `Episode`: lifecycle compartido por `priority_group`.
- `ManagementEffect`: efecto temporal de una acción de Management.
- `DeactivationIntent`: intención operacional de deactivation ya materializada antes del Core.
- `CascadeSuppression`: supresión operacional derivada de un ManagementEffect.
- `ReappearanceChange`: reaparición operacional de la misma occurrence gestionada.
- `AlarmEffectiveConfigurationHead`: read model durable de la resolución global actualmente EFFECTIVE.

## Identidad

```text
AlarmIdentity(family_key, alarm_key)
```

`alarm_key` es estable.

No introducir `rule_key` paralelo.

Family y `priority_group` son conceptos distintos.

## AlarmResolutionKey

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
AlarmResolutionKey
    alarm_configuration_revision
    confirmed_tool_catalog_revision
```

Es una identidad de configuración compartida por:
- B.2 Materialization;
- Runtime Configuration;
- Delivery Configuration;
- Runtime Adoption;
- Effective Configuration Head;
- Delivery;
- Management Capture.

No agregar `evaluator_registry_revision` sin un contrato real de esa provenance.

## AlarmDefinition, PlannedAlarm y evaluator

La frontera target es:

```text
AlarmDefinition
    evaluator_key
    parameters
    business/runtime configuration
        |
        v
B.2 Configuration Resolution
        |
        v
PlannedAlarm
    evaluator_key
    runtime policy resolved
    NO evaluator callable
        |
        + parameters
        |
        v
Runtime
        +
deployed AlarmEvaluatorRegistry
        |
        v
AlarmExecutionEntry
        |
        v
Engine
```

`PlannedAlarm` no contiene la lógica Python que evalúa la condición.

B.2 valida que `(family_key, evaluator_key)` exista, pero no serializa ni transporta callables, `DataRequirements`, `DataLoadPlan` ni `AlarmExecutionSession` como artifact de configuración.

Runtime vuelve a resolver el evaluator desplegado antes de construir la execution session.

## PlannedAlarm CURRENT

CURRENT implementado contiene, entre otros:

```text
identity
kind
criticality
priority_group
priority_order
delivery_enabled
evaluator_key
alarm_configuration_revision
tool_registry_revision
routing
deactivation_policy
reappearance_special_conditions
```

`reappearance_special_conditions` es `tuple[AlarmIdentity, ...]` y no admite duplicados.

## PlannedAlarm TARGET

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
PlannedAlarm
    identity
    kind
    criticality
    priority_group
    priority_order
    evaluator_key
    resolution_key
    routing
    reappearance_after_seconds: int | None
    reappearance_special_conditions: tuple[AlarmIdentity, ...]
```

La shape exacta puede distribuir provenance fuera de `PlannedAlarm` si el Runtime artifact ya la garantiza globalmente, pero la semántica es única: no mantener dos revisions históricas como contrato paralelo.

Quedan fuera del target `PlannedAlarm`:

```text
delivery_enabled
deactivation_policy
visibility_mode
Message metadata
configured deactivation max
```

Razones:
- visibility pertenece a Delivery;
- deactivation policy es contextual al Message/gestión y se resuelve antes del Engine;
- `max_duration_hours` y `enabled` no son decisiones lifecycle del Core.

## Runtime provenance — cleanup target

CURRENT todavía usa:

```text
alarm_configuration_revision
tool_registry_revision
```

TARGET:

```text
resolution_key: AlarmResolutionKey
```

La implementación debe reemplazar limpiamente el naming histórico, sin alias permanentes ni doble provenance paralela.

Una occurrence conserva provenance histórica del momento en que nació:

```text
resolution_key_at_start
```

No se reescribe cuando cambia la configuración EFFECTIVE.

## Priority

Invariantes CURRENT:

```text
priority_order > 0
priority_order único dentro del priority_group
menor priority_order = mayor prioridad
```

El Engine todavía valida IMPACT-before-RISK cuando ambos kinds existen en el mismo grupo.

Management suppression CURRENT ya se gobierna por `priority_order`, independiente de `kind`.

## Visibility — target

B.1 congeló:

```text
VISIBLE
TRACE_ONLY
```

`TRACE_ONLY`:
- continúa evaluándose;
- continúa trazándose;
- participa normalmente de lifecycle, routing, priority y management;
- no se publica como alarma operacional visible por Delivery.

CURRENT `delivery_enabled=false` produce `SHADOW` y además altera priority/Management. Por tanto:

```text
TRACE_ONLY != delivery_enabled=false
```

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
PlannedAlarm.delivery_enabled
-> REMOVE

PriorityDisposition.SHADOW
-> REMOVE como target Runtime
```

No reemplazar `delivery_enabled` por un nuevo flag de visibilidad dentro del Engine.

Una Rule TRACE_ONLY predominante puede dejar el grupo sin alarma visible; Delivery no promueve una Rule visible eclipsada porque priority ya fue resuelto por Engine.

## Management suppression

CURRENT implementado:

```text
managed source priority = P

target.priority_order < P
-> no suppression

target.priority_order > P
-> eligible para CascadeSuppression
```

TARGET elimina cualquier filtro por `delivery_enabled`/visibility.

Visibility no altera suppression.

Management suppression:
- no cierra occurrence target;
- no cambia condición física;
- no detiene routing;
- se libera cuando el ManagementEffect deja de tener alcance.

## Deactivation — boundary target

CURRENT Engine consulta `PlannedAlarm.deactivation_policy.approval_required` al procesar una acción.

Esto no soporta correctamente overrides por Message ni preserva la policy seleccionada si la configuración cambia antes del consumo de la acción.

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
PlannedAlarm.deactivation_policy
-> REMOVE
```

Management Capture resuelve la policy efectiva usando la Delivery Configuration EFFECTIVE y crea:

```text
DeactivationIntent
    effective_until
    approval_required
```

No llegan al Engine:
- `enabled`;
- `max_duration_hours`;
- `message_key`;
- `shift_end`;
- `operator_selected_until`.

El durable `DeactivationRequest` conserva `effective_until + approval_required`, de modo que cambios posteriores de configuración no reinterpretan la solicitud.

## Special Condition

En Alarm Configuration, Special Condition es una Rule marcada explícitamente:

```text
is_special_condition=true
```

Engine no necesita ese flag; consume referencias calificadas en:

```text
reappearance_special_conditions
```

B.2 qualification exige, para cada referencia:

```text
referenced Rule exists
AND is_special_condition == true
AND same family
AND same priority_group
AND not self-reference
```

Self-reference queda BLOCKING porque con trigger level-triggered produciría una reappearance tautológica inmediata.

Una Special Condition válida pero `is_active=false` sigue siendo una referencia válida; simplemente no está ejecutándose y por tanto no puede estar ACTIVE.

## Reappearance CURRENT

CURRENT:

```text
ManagementEffect.reappearance_due_at: datetime
```

y Runtime recibe un `ReappearanceDueAtResolver` global.

Special Condition reappearance está implementada y validada:

```text
managed A
AND same occurrence
AND A ACTIVE
AND any referenced SC ACTIVE
-> reappearance
```

Semántica OR, level-triggered; INACTIVE/ERROR no disparan; una occurrence cerrada no se resucita; timer + SC en el mismo ciclo produce una sola reappearance.

## Reappearance TARGET

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
AlarmDefinition.reappearance.after_minutes
        |
        | * 60
        v
PlannedAlarm.reappearance_after_seconds: int | None
```

Y:

```text
ManagementEffect.reappearance_due_at: datetime | None
```

`None` significa ausencia de timer. Combinaciones válidas:

```text
None + no SC        -> sin reappearance automático
timer + no SC       -> sólo timer
None + SC           -> sólo Special Condition
timer + SC          -> timer OR Special Condition
```

`ReappearanceDueAtResolver` global queda SUPERSEDED como target.

El due se deriva de:

```text
management_effect.effective_at
+
planned_alarm.reappearance_after_seconds
```

Un cambio de timer durante una gestión activa se reconcilia desde el `effective_at` original. Si el nuevo due teórico ya venció, la reappearance ocurre en el instante efectivo de Adoption, no retroactivamente.

Cambiar a `None` cancela sólo el mecanismo temporal; Special Conditions pueden seguir disparando.

## Effective configuration

La configuración EFFECTIVE global no se infiere desde snapshots individuales.

PROJECT CONTRACT AGREED:

```text
AlarmEffectiveConfigurationHead
    resolution_key
    effective_at
    adoption_id
```

`GroupRuntimeSnapshot.state_basis` conserva provenance de la última mutación de ese hot state y puede ser anterior al Effective Head sin ser inconsistente.

Ver `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md`.

## Conflicto con B.1 FROZEN

B.1 frozen describió una Special Cascade donde una Special Condition gestionada y predominante podía bloquear todas las demás Rules activas del scope.

La implementación CURRENT usa una única semántica de Management suppression gobernada por `priority_order`, independiente de `kind` e independiente del flag Special Condition.

`is_special_condition` conserva semántica específica de reappearance.

Estado:

```text
IMPLEMENTATION CURRENT / VALIDATED
PROJECT REFINEMENT AGREED
DECISIONS REPOSITORY NOT YET RECONCILED
CONFLICT MUST REMAIN VISIBLE
```

No declarar B.1 formalmente superseded hasta registrar el refinamiento en `atlanticus-decisions`.
