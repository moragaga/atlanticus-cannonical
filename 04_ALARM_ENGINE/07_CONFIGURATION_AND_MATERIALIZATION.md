# Alarm Engine — Configuration and Materialization

Estado: **B.2 IN PROGRESS / RESOLUTION + RUNTIME/DELIVERY ARTIFACTS + ADOPTION + LIVE DELIVERY CONTRACT AGREED / NOT YET IMPLEMENTED**

## Authority checkpoint

Implementación auditada:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Canonical base de este delta:

```text
moragaga/atlanticus-cannonical:main
3ffa87c0e4249d749af4e669a977dfd744a666bb
```

Decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Este documento distingue explícitamente:

```text
CURRENT / VERIFIED
DECISION RECORDED
PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED
OPEN
```

## Invariantes centrales

```text
LATEST SAVED = LATEST VALID_AT_SAVE
```

Una revisión persistida pasó full PRE-SAVE validation en el instante de persistencia.

Eso no implica que permanezca materializable para siempre:

```text
VALID_AT_SAVE
!=
READY_AT_ANY_LATER_TIME
!=
EFFECTIVE
```

Una dependencia externa puede derivar después del save. La revisión histórica sigue siendo legítima, pero Materialization puede quedar `BLOCKED` y la revisión EFFECTIVE anterior se conserva.

Invariantes centrales:

```text
INVALID != REMOVED
DISABLED != INVALID
TRACE_ONLY != REMOVED
READY != EFFECTIVE
```

B.2 nunca elimina silenciosamente una Rule rota para producir un artifact parcial.

## CURRENT antes de implementar B.2

Alarm Configuration ya dispone de:
- aggregate durable Rules + Messages;
- validaciones locales/cross-rule;
- Source/Release;
- base Projection exacta;
- Manager/history;
- structured authoring;
- Tool Catalog V1;
- Alarm Tool Reference read model.

Runtime ya dispone de:
- `PlannedAlarm`;
- `AlarmExecutionSession`;
- `AlarmEvaluatorRegistry`;
- adoption/reconciliation;
- routing C1/C2/C3;
- persistence/recovery y hot runtime snapshots.

No existe todavía un owner concreto que implemente el B.2 acordado en este documento.

## Tres capas de validación

```text
PRE-SAVE VALIDATION
-> antes de persistir Alarm Configuration
-> valida el candidato completo
-> blocking finding impide nueva revisión

MATERIALIZATION VALIDATION / B.2
-> vuelve a validar revisión persistida + dependencias actuales
-> detecta drift posterior al save
-> produce READY o BLOCKED

RUNTIME ADOPTION
-> aplica transición desde el estado EFFECTIVE actual
-> decide si la revisión READY puede pasar a EFFECTIVE
```

Regla transversal:

```text
LOCAL VALIDATION
-> tipo, forma, rango e invariantes intrínsecos

B.2 VALIDATION
-> relaciones entre campos
-> relaciones entre Rules
-> referencias externas
-> semántica cross-contract

RUNTIME ADOPTION
-> transición sobre occurrences/episodes/estado durable existente
```

B.2 no corrige silenciosamente configuración inválida; produce findings.

## Inputs de Resolution

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
Published Alarm Configuration Release
+ Confirmed Tool Catalog
+ current Tool reconciliation qualification
+ deployed evaluator catalog/registry for qualification
        |
        v
B.2 Configuration Resolution
```

La adquisición pertenece al Materialization Job.

Runtime no debe convertirse en:
- SharePoint downloader;
- Tool discovery process;
- Message resolver;
- cross-configuration validator.

## Resolution identity / provenance

```text
AlarmResolutionKey
    alarm_configuration_revision
    confirmed_tool_catalog_revision
```

`AlarmResolutionKey` pasa a ser un value object contractual compartido por Materialization, Runtime, Delivery, Adoption y Management Capture.

No se agrega `evaluator_registry_revision` porque no existe un contrato CURRENT que lo sustente.

## Resultado de B.2

```text
AlarmConfigurationResolution
    resolution_key
    status: READY | BLOCKED
    findings: tuple[AlarmResolutionFinding, ...]
    runtime_configuration: RuntimeAlarmConfiguration | None
    delivery_configuration: DeliveryAlarmConfiguration | None
```

### READY

```text
READY
<=> no existe BLOCKING finding
<=> runtime_configuration existe
<=> delivery_configuration existe
<=> ambos artifacts comparten exactamente el mismo resolution_key
```

### BLOCKED

```text
BLOCKED
<=> existe al menos un BLOCKING finding
<=> runtime_configuration is None
<=> delivery_configuration is None
<=> EFFECTIVE no avanza
```

No existe publicación parcial por Rule ni readiness independiente por capability.

## Findings

```text
AlarmResolutionFinding
    code
    severity: BLOCKING | WARNING
    message
    alarm_identity?
    field_path?
    reference_key?
```

`BLOCKING` cubre cualquier inconsistencia que pueda alterar Runtime, adoption, routing, management, deactivation o Delivery correcto.

`WARNING` queda reservado a calidad administrativa no contractual.

B.2 recolecta findings independientes cuando sea seguro hacerlo, evitando cascadas derivadas de una misma dependencia ausente.

## Atomicidad de la revisión

Una sola Rule inválida bloquea la resolución completa.

Prohibido:

```text
A valid
B invalid
C valid
-> artifact = A + C
```

porque Runtime podría interpretar B como `REMOVED`.

## Acquisition failure != B.2 BLOCKED

Si el Materialization Job no puede adquirir inputs confiables, no existe un candidato resoluble completo.

Eso es failure/diagnostic del job, no una `AlarmConfigurationResolution(BLOCKED)` inventada.

## Runtime Configuration Artifact

```text
RuntimeAlarmConfiguration
    resolution_key
    defined_alarm_identities
    planned_alarms
    parameters_by_alarm
```

Debe ser serializable.

No contiene:
- evaluator callable;
- `DataRequirement` como artifact persistido;
- `DataLoadPlan`;
- `AlarmExecutionSession`;
- lifecycle hot state;
- Message Catalog;
- visibility metadata de Delivery.

### defined vs executable

```text
Rule active
-> identity in defined_alarm_identities
-> PlannedAlarm presente

Rule disabled
-> identity in defined_alarm_identities
-> PlannedAlarm ausente

Rule removed
-> identity ausente
```

Una Rule disabled sigue siendo validada completamente.

## Evaluator boundary

B.2 valida `(family_key, evaluator_key)` para todas las Rules definidas, incluidas disabled.

El artifact conserva la referencia pero no el código.

Runtime une:

```text
RuntimeAlarmConfiguration
+ deployed AlarmEvaluatorRegistry
-> AlarmExecutionSession
```

La API concreta del registry para qualification B.2 sigue por implementar.

## Routing materialization

### Runtime semantics VERIFIED

Engine CURRENT interpreta:
- C1: origin + destinations inmediatos;
- C2: origin inmediato + `delay_seconds` absolutos desde `occurrence.started_at`;
- C3: origin only.

Management no detiene el reloj de routing.

### Orden

B.2 ordena steps por `step_order`; el orden físico de la tuple no es semántico; no se exige secuencia contigua.

### C1

```text
origin -> inmediato
enabled step -> delay_seconds=None
enabled step + wait > 0 -> BLOCKING
```

`None` o `0` son coherentes con C1. Un step disabled no genera destination.

### C2

Todos los steps, enabled o disabled, requieren:

```text
wait_minutes_from_previous_step is not None
wait_minutes_from_previous_step >= 0
```

B.2 acumula todos los waits ordenados y sólo emite destinations enabled.

Ejemplo:

```text
step 1 enabled  wait 15
step 2 disabled wait 20
step 3 enabled  wait 30
```

materializa:

```text
step 1 -> 900 sec
step 3 -> 3900 sec
```

Deshabilitar un destino no adelanta silenciosamente destinos posteriores.

### C3

```text
origin only
```

Cualquier step enabled produce BLOCKING. Steps disabled pueden permanecer pero siguen sujetos a validación contractual y de referencias.

### Tool references

Se validan origin y todos los targets, incluidos disabled:

```text
exists in Confirmed Tool Catalog
AND current reconciliation qualification is GREEN
AND Tool kind is alarm-eligible
```

`STRATEGIC` no es elegible como Alarm Configuration Tool reference.

OPEN:
- restricciones PROCESS ↔ INTEGRATED_OPERATIONS;
- routing tier matrix.

## Deactivation + Messages materialization

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED.

Authoring conserva:

```text
AlarmDefinition.default_deactivation
    enabled
    max_duration_hours
    approval_required

MessageDefinition.deactivation_override?
    enabled
    max_duration_hours
    approval_required
```

Precedencia frozen:

```text
message override absent
-> Rule default

message override present
-> reemplazo completo del default
```

B.2 materializa una policy estática resuelta:

```text
ResolvedDeactivationPolicy
    enabled
    max_duration_hours
    approval_required
```

No contiene `effective_until`.

Para cada Rule, Delivery Configuration debe poder representar:
- default deactivation policy resuelta;
- Messages activos seleccionables;
- policy efectiva resuelta de cada Message.

Un Message `is_active=false` sigue siendo validado pero no se ofrece para nuevas gestiones.

Sin Message contextual se usa el default de la Rule.

Seleccionar un Message no desactiva automáticamente nada.

## Management Capture boundary

Management Capture usa exclusivamente la configuración EFFECTIVE exacta.

Solicitud conceptual:

```text
ManagementSubmission
    alarm_identity
    source_occurrence_id
    tool_key
    resolution_key
    message_key?
    requested_deactivation_until?
```

`resolution_key + source_occurrence_id` impiden reinterpretar una acción contra una revisión u occurrence posteriores.

`message_key` es provenance de Management, no un campo de `PlannedAlarm` ni del Engine Core.

Si se solicita deactivation, Capture usa:

```text
resolved deactivation policy
+ operator requested until
+ source_created_at
+ shift_end
```

para calcular el contrato B.1:

```text
effective_until = min(
    operator_selected_until,
    source_created_at + configured_max_duration,
    shift_end,
)
```

El origen concreto de `shift_end` sigue OPEN.

Target Engine input:

```text
DeactivationIntent
    effective_until
    approval_required
```

Por tanto:

```text
PlannedAlarm.deactivation_policy
-> REMOVE target
```

El durable `DeactivationRequest` conserva la policy efectiva capturada.

## Special Condition qualification

B.2 transforma referencias authoring hacia `PlannedAlarm.reappearance_special_conditions` sólo después de qualification.

Para cada referencia:

```text
referenced Rule exists
AND referenced Rule.is_special_condition == true
AND same family
AND same priority_group
AND referenced identity != source identity
```

Findings conceptuales BLOCKING:

```text
reappearance_special_condition_not_found
reappearance_target_not_special_condition
reappearance_special_condition_family_mismatch
reappearance_special_condition_priority_group_mismatch
reappearance_special_condition_self_reference
```

Una Special Condition válida pero disabled sigue siendo una referencia válida; al no ser executable no puede estar ACTIVE.

## Reappearance timer materialization

B.1 authoring usa minutos:

```text
after_minutes: int | None
```

B.2 target Runtime usa:

```text
PlannedAlarm.reappearance_after_seconds: int | None
```

Conversión:

```text
after_minutes * 60
```

No se define máximo artificial.

Target `ManagementEffect`:

```text
reappearance_due_at: datetime | None
```

`None` representa ausencia de timer.

Combinaciones válidas:

```text
None + no SC   -> sin reappearance automático
timer + no SC  -> sólo timer
None + SC      -> sólo Special Condition
timer + SC     -> timer OR Special Condition
```

El `ReappearanceDueAtResolver` global CURRENT queda SUPERSEDED como target. El due se deriva de la policy de la Rule y del `ManagementEffect.effective_at` original.

Runtime Adoption reconcilia cambios de timer sobre ManagementEffects abiertos:
- timer mayor/menor recalcula desde `effective_at` original;
- nuevo due ya vencido -> reappearance en `adoption effective_at`;
- timer -> `None` cancela sólo el trigger temporal;
- `None` -> timer puede producir reappearance inmediata si el due teórico ya venció.

Cambios de Special Condition refs son compatibles; por ser level-triggered, una nueva SC ya ACTIVE puede disparar en el primer ciclo bajo la nueva configuración.

## Visibility materialization

B.1:

```text
VISIBLE
TRACE_ONLY
```

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
visibility_mode
-> Delivery Configuration only
```

No llega al Runtime artifact.

CURRENT:

```text
delivery_enabled=false
-> SHADOW
-> altera priority/Management/suppression
```

TARGET:

```text
PlannedAlarm.delivery_enabled
-> REMOVE

PriorityDisposition.SHADOW
-> REMOVE
```

No sustituirlos por otro flag de visibility en Core.

TRACE_ONLY participa normalmente de lifecycle, evaluation, routing, priority, management y suppression. Delivery únicamente decide si el predominant ya resuelto se publica visiblemente.

Una Rule TRACE_ONLY predominante no promueve a una visible eclipsada.

## Runtime Adoption y EFFECTIVE

Materialization responde:

```text
¿el candidato es coherente y puede producir artifacts READY?
```

Runtime Adoption responde:

```text
¿cómo transiciona el estado operacional EFFECTIVE actual al target READY?
```

Por tanto:

```text
B.2 READY
-> Runtime Adoption REJECTED
-> EFFECTIVE permanece anterior
```

La semántica global de Adoption/EFFECTIVE queda consolidada en:

```text
13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md
```

Incluye:
- `AlarmEffectiveConfigurationHead`;
- Adoption durable incluso con cero group-state mutations;
- same-WAL atomicity/recovery;
- `ADDED` y `ENABLED`;
- clasificación sobre union de defined identities;
- exact-key consumption por Delivery/Management Capture.

## Delivery Configuration Artifact

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED.

Una resolución READY produce también:

```text
DeliveryAlarmConfiguration
    resolution_key: AlarmResolutionKey
    alarms: tuple[ResolvedDeliveryAlarm, ...]
```

El artifact representa todas las Rules definidas de esa revisión válida, incluidas disabled y TRACE_ONLY. Una Rule removida está ausente.

Por Rule materializa sólo configuración de Delivery/Management ya resuelta:

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
resolved active Messages
resolved visual targets
```

No contiene evaluator, parameters, priority source data, routing, reappearance, lifecycle state ni ToolStructure completo.

Messages activos seleccionables quedan materializados como:

```text
ResolvedDeliveryMessage
    message_key
    display_text
    deactivation_policy: ResolvedDeactivationPolicy
```

El consumidor no vuelve a aplicar override precedence.

Visual targets quedan resueltos como direcciones estables sobre la revisión exacta del Tool Catalog:

```text
tool_key
tool_kind
component_keys
(owner_component_key, subcomponent_key)
process_projection_mode?
```

B.2 no copia `ToolStructure`; Tool Configuration conserva ownership de topología y metadata estructural.

## Engine resolved current-state output

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED.

Live Delivery no lee WAL, Evidence History ni `GroupRuntimeSnapshot` como API operacional. Runtime debe exponer una salida explícita después de cada ciclo exitoso:

```text
EngineResolvedCurrentState
    resolution_key
    as_of
    alarms: tuple[ResolvedCurrentAlarmState, ...]
```

Incluye sólo occurrences abiertas y, por occurrence:

```text
identity
occurrence_id
episode_id
started_at
current AlarmEvaluation / EvidenceSnapshot
resolved priority disposition
technical hold?
management state?
deactivation state?
pending deactivation request?
assignments
pending assignments
```

El resultado completo del ciclo ya conserva `AlarmEvaluation`, por lo que la evidencia actual no necesita recuperarse desde sampling histórico ni persistirse por conveniencia dentro del hot snapshot.

Un ciclo sin lifecycle mutation puede igualmente producir nuevo Current State cuando cambian los valores evaluados. Si el ciclo requiere commit durable, la salida Live se construye sólo después de confirmar ese commit.

## Cause materialization

`cause_template` es configuración estática. El Web no debe interpretarlo.

Live Delivery materializa:

```text
cause_template
+ current AlarmEvaluation.evidence_snapshot.payload
-> cause_text
```

Estados conceptuales acordados:

```text
RESOLVED
TECHNICAL_UNAVAILABLE
MATERIALIZATION_ERROR
```

Durante `ERROR`/technical hold no existe current physical evidence; no se presenta silenciosamente el último valor conocido como actual.

Un fallo de formateo de cause no debe ocultar una occurrence operacional real: la occurrence puede publicarse con `MATERIALIZATION_ERROR` y diagnóstico, sin inventar texto.

La validación estática exacta `cause_template <-> evaluator evidence schema` permanece OPEN porque CURRENT no expone un schema contractual suficiente de placeholders por evaluator.

## Alarm Live Projection publication contract

La unión exige exact alignment:

```text
EngineResolvedCurrentState.resolution_key
== DeliveryAlarmConfiguration.resolution_key
== AlarmEffectiveConfigurationHead.resolution_key
```

Para cada occurrence abierta:

```text
PUBLISH
IFF visibility_mode == VISIBLE
AND priority_disposition IN {PREDOMINANT, DEACTIVATED}
```

Por tanto:
- `PREDOMINANT + VISIBLE` -> publicar;
- `DEACTIVATED + VISIBLE` -> publicar;
- `ECLIPSED` -> no publicar;
- `CASCADE_SUPPRESSED` -> no publicar;
- `TRACE_ONLY` -> no publicar cualquiera sea la disposición;
- una TRACE_ONLY predominante no promueve una visible eclipsada;
- Management y technical hold son atributos del current state, no filtros independientes de publicación.

`PriorityDisposition.SHADOW` queda fuera del target junto con `PlannedAlarm.delivery_enabled`.

El Live snapshot lógico es completo para un único `resolution_key + as_of`. Un snapshot vacío es válido. No se publica parcialmente por priority group.

La definición consolidada del artifact Delivery, Engine Current State, cause materialization, publication rules y Management round-trip vive en:

```text
../14_ADA_COMMAND_CENTER/16_ALARM_LIVE_DELIVERY_CONTRACT.md
```

## OPEN después de este checkpoint

Permanece OPEN:
- owner/package concreto de B.2 y Live Delivery;
- input contractual de current Tool reconciliation GREEN;
- validación estática `cause_template <-> evaluator evidence schema`;
- proveedor concreto de `shift_end`;
- persistencia física del Captured Management Input;
- cleanup/invalidation autónoma de pending deactivation requests stale;
- provenance rename en implementación;
- restricciones adicionales de Tool kinds/tier routing;
- adoption gaps C1/C3/evaluator/kind/priority-group/origin Tool;
- journal discriminated record schema/adoption persistence implementation;
- migration desde persistence CURRENT;
- schema versions/codecs físicos, cadence, persistence y retention de artifacts/Live Projection.

## No reabrir Engine por conveniencia

B.2 debe calificar/materializar. No debe:
- reimplementar priority;
- reimplementar Management suppression;
- reimplementar Special Condition Runtime reappearance;
- introducir UI semantics dentro de Core;
- conservar aliases legacy o adapters temporales.
