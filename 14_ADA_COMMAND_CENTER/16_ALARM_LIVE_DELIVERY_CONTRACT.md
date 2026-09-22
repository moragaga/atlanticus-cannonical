# ADA Command Center — Alarm Live Delivery Contract

Estado: **PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED**

## 1. Authority checkpoint

Implementación CURRENT auditada:

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

Este documento distingue:

```text
VERIFIED / CURRENT
DECISION RECORDED
PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED
OPEN
```

## 2. Propósito y ownership

Este contrato consolida la frontera:

```text
B.2 Delivery Configuration
+
Runtime resolved current state
        |
        v
Live Delivery
        |
        v
Alarm Live Projection
        |
        v
Operational Web / Management Capture
```

Live Delivery enriquece estado operacional ya resuelto. No es un segundo Engine.

No debe:
- descubrir Tools;
- volver a SharePoint;
- resolver Message precedence;
- evaluar Rules;
- recalcular lifecycle;
- recalcular priority;
- recalcular routing;
- leer WAL como API operacional.

## 3. Alignment con EFFECTIVE

Toda unión operacional exige:

```text
EngineResolvedCurrentState.resolution_key
== DeliveryAlarmConfiguration.resolution_key
== AlarmEffectiveConfigurationHead.resolution_key
```

No se usa:
- latest READY;
- highest revision;
- fallback a otra revisión.

Si falta el artifact exacto, no se reinterpretan datos bajo otra configuración.

## 4. DeliveryAlarmConfiguration

PROJECT CONTRACT AGREED:

```text
DeliveryAlarmConfiguration
    resolution_key: AlarmResolutionKey
    alarms: tuple[ResolvedDeliveryAlarm, ...]
```

`alarms` contiene todas las Rules definidas de la revisión válida:

```text
active      -> present
disabled    -> present with is_active=false
TRACE_ONLY  -> present
removed     -> absent
```

Esto preserva:

```text
DISABLED != REMOVED
TRACE_ONLY != REMOVED
```

Orden serializable recomendado: `AlarmIdentity`.

## 5. ResolvedDeliveryAlarm

Shape conceptual acordado:

```text
ResolvedDeliveryAlarm
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

No se proyectan por conveniencia:

```text
rule_name
is_special_condition
evaluator_key
parameters
priority_group
priority_order
reappearance
escalation
AlarmRouting
callables / DataRequirements / DataLoadPlan
hot lifecycle state
```

En particular, Delivery no recibe `priority_order`: no debe poder reconstruir predominancia.

## 6. Resolved Messages y deactivation capability

Policy estática ya acordada:

```text
ResolvedDeactivationPolicy
    enabled
    max_duration_hours
    approval_required
```

Messages operacionalmente seleccionables:

```text
ResolvedDeliveryMessage
    message_key
    display_text
    deactivation_policy: ResolvedDeactivationPolicy
```

B.2 ya aplicó:

```text
override absent  -> Rule default
override present -> full replacement
```

El consumer nunca repite esa precedencia.

Un Message `is_active=false` sigue siendo validado, pero no aparece como opción para nuevas gestiones.

`message_keys=()` continúa siendo válido.

## 7. Visual targets resueltos

Tool Configuration conserva ownership de estructura/topología. `DeliveryAlarmConfiguration` no copia `ToolStructure`.

Target conceptual:

```text
ResolvedAlarmVisualTarget
    tool_key
    tool_kind
    component_keys
    subcomponents
        owner_component_key
        subcomponent_key
    process_projection_mode?
```

`tool_kind` materializa la clasificación que B.2 ya tuvo que resolver. El universo actual elegible es:

```text
PROCESS
INTEGRATED_OPERATIONS
```

`STRATEGIC` permanece fuera mientras Tool Configuration no defina Alarm Projection para ese kind.

No se duplican:
- Tool display name;
- SourceReleaseId por Tool;
- ToolStructure;
- component/subcomponent display names;
- linked-component topology;
- layout role.

Para linked subcomponents se conserva la dirección inequívoca `(owner_component_key, subcomponent_key)`.

## 8. Routing y visual projection siguen separados

`visual_targets` y Runtime `assignments/pending_assignments` son hechos distintos.

No existe decisión vigente que autorice:

```text
visual target visible IFF Tool assigned
```

Por tanto este contrato no inventa esa intersección. Si una superficie futura la necesita, deberá decidirse explícitamente.

## 9. EngineResolvedCurrentState

PROJECT CONTRACT AGREED:

```text
EngineResolvedCurrentState
    resolution_key: AlarmResolutionKey
    as_of: datetime
    alarms: tuple[ResolvedCurrentAlarmState, ...]
```

Contiene sólo occurrences abiertas al terminar el ciclo.

Por occurrence:

```text
ResolvedCurrentAlarmState
    identity
    occurrence_id
    episode_id
    started_at

    evaluation
    priority

    technical_hold?
    management_cycle
    management_effect?
    deactivation_effect?
    pending_deactivation_request?

    assignments
    pending_assignments
```

No es un reemplazo durable de `GroupRuntimeSnapshot`; es una salida operacional resuelta.

## 10. Evaluación actual y evidence

VERIFIED CURRENT:

`AlarmOperationalCycleResult` ya conserva las `AlarmEvaluation` completas del ciclo junto con los `GroupLifecycleDecision` resueltos.

Una evaluación física contiene:

```text
EvidenceSnapshot
    contract_key
    contract_version
    payload
```

El hot `RuntimeEvaluationState` CURRENT conserva sólo status/timestamp/error key y no es suficiente para Live content.

Por tanto, `EngineResolvedCurrentState` usa la evaluación completa del ciclo, no reconstruye evidence desde history sampling ni convierte el WAL en read API.

Una evaluación `ACTIVE -> ACTIVE` puede cambiar valores sin producir lifecycle mutation. Aun así el nuevo Current State debe reflejar los valores actuales.

## 11. Momento de producción del Current State

Orden lógico target:

```text
evaluate
-> reduce lifecycle
-> finalize management/deactivation
-> resolve routing
-> resolve priority
-> persist required Engine changes
-> commit confirmed
-> build EngineResolvedCurrentState
-> Live Delivery
```

Si el ciclo no requiere commit durable, puede igualmente producir un nuevo Current State.

No se publica una salida derivada de un commit requerido que todavía no fue confirmado.

## 12. Cause materialization

`cause_template` es configuración estática y no representa por sí solo el contenido visible.

Live Delivery resuelve:

```text
cause_template
+ current EvidenceSnapshot.payload
-> cause_text
```

El Web consume `cause_text`; no interpreta placeholders.

Shape conceptual:

```text
LiveCause
    status: RESOLVED | TECHNICAL_UNAVAILABLE | MATERIALIZATION_ERROR
    text: str | None
```

### RESOLVED

Current physical evidence contiene los valores requeridos y el backend materializa el texto.

### TECHNICAL_UNAVAILABLE

Durante `AlarmEvaluation.status=ERROR` / technical hold no existe current `EvidenceSnapshot`. No se presenta silenciosamente un valor físico anterior como actual.

### MATERIALIZATION_ERROR

La occurrence es operacionalmente real pero el template no puede materializarse con el payload actual.

Regla acordada:

```text
content materialization failure != hide operational alarm
```

La occurrence permanece publicable según visibility/priority y se emite diagnóstico.

OPEN: CURRENT no dispone de un schema evaluator-evidence suficientemente explícito para validar estáticamente todos los placeholders de `cause_template`.

## 13. Technical hold

CURRENT permite una occurrence abierta mientras la evaluación está `ERROR` durante el grace period.

Si su disposición sigue siendo publicable, permanece en Live con:

```text
evaluation_status = ERROR
technical_hold.started_at
technical_hold.due_at
cause.status = TECHNICAL_UNAVAILABLE
```

Technical hold no equivale a normalización física.

## 14. Priority publication rule

DECISION RECORDED + PROJECT CONTRACT AGREED:

```text
PUBLISH occurrence
IFF
    occurrence is open
    AND matching Delivery Rule exists
    AND visibility_mode == VISIBLE
    AND priority_disposition IN {PREDOMINANT, DEACTIVATED}
```

Matriz:

| Engine disposition | VISIBLE | TRACE_ONLY |
|---|---|---|
| `PREDOMINANT` | publish | omit |
| `DEACTIVATED` | publish | omit |
| `ECLIPSED` | omit | omit |
| `CASCADE_SUPPRESSED` | omit | omit |

Target elimina `PriorityDisposition.SHADOW` junto con `PlannedAlarm.delivery_enabled`.

Una TRACE_ONLY predominante no causa promoción de una visible eclipsada.

## 15. Managed y deactivated en Live

Management no redefine la condición física.

Una occurrence `PREDOMINANT` puede publicarse aunque tenga `ManagementEffect`; Management es un atributo actual.

Una occurrence físicamente activa con `DEACTIVATED` también permanece en Live. Esto permite representar simultáneamente, por ejemplo:

```text
A ACTIVE / DEACTIVATED
B ACTIVE / PREDOMINANT
```

Ambas pueden existir en la misma Live Projection si son `VISIBLE`. Web no recalcula cuál es predominante.

`ECLIPSED` y `CASCADE_SUPPRESSED` no se entregan como alarmas operacionales actuales.

## 16. Current management/deactivation fields

Current State expone, cuando existen:

```text
CurrentManagementState
    effect_id
    effective_at
    reappearance_due_at?

CurrentDeactivationState
    effect_id
    effective_from
    effective_until
```

`management_cycle` pertenece al current occurrence.

La configuración estática de max duration/approval no se duplica en estos objetos; vive en Delivery Configuration para futuras acciones.

## 17. Pending deactivation request

CURRENT Runtime recibe pending durable requests fuera de `AlarmRuntimeState`. Son estado operacional relevante para Web.

El Current State debe asociar una request sólo cuando:

```text
request.alarm_identity == current identity
AND request.source_occurrence_id == current occurrence_id
```

Shape mínimo:

```text
CurrentPendingDeactivationRequest
    request_id
    requested_at
    effective_until
```

Esto permite expresar `PENDING_APPROVAL` y evitar una segunda solicitud innecesaria.

Una request de una occurrence anterior no se adjunta a la nueva occurrence.

OPEN: cleanup/invalidation autónoma de pending requests que quedan stale sin recibir decisión.

## 18. AlarmLiveProjection

Target conceptual:

```text
AlarmLiveProjection
    resolution_key
    as_of
    occurrences: tuple[AlarmLiveOccurrence, ...]
```

`AlarmLiveOccurrence` contiene información ya resuelta para Web:

```text
resolution_key
identity
occurrence_id
episode_id
started_at
evaluated_at

evaluation_status
priority_disposition

display_name
title
cause: LiveCause

kind
criticality
business_category
operational_areas
color

technical_hold?
management state
deactivation state
pending deactivation request?

assignments
pending_assignments

messages
default_deactivation_policy
visual_targets
```

El Web no necesita `cause_template`, `priority_order`, Message overrides, Tool catalogs ni AlarmDefinition.

## 19. Snapshot semantics

Un Live snapshot representa un único:

```text
resolution_key + as_of
```

La materialización lógica es completa, no parcial por group.

Un snapshot vacío es válido cuando no hay occurrences publicables.

Si existe una occurrence que debería materializarse pero falta su Rule en la `DeliveryAlarmConfiguration` exacta, no se elimina silenciosamente la occurrence para continuar. La nueva materialización falla con diagnóstico.

Un error sólo de cause es diferente: puede expresarse como `MATERIALIZATION_ERROR` sin perder la occurrence.

## 20. Web filtering vs business decisions

Web puede filtrar la proyección ya resuelta, por ejemplo:

```text
Active Alarm surface
Deactivated filter
Managed filter
layout by visual targets
```

Eso no autoriza a Web a:
- ordenar `priority_order`;
- promover eclipsed Rules;
- calcular cascade suppression;
- resolver routing;
- resolver Message precedence;
- recalcular deactivation capability.

## 21. Management round-trip

La proyección Live entrega la identidad y provenance necesarias para una gestión, pero el browser devuelve intención, no una copia autoritativa de la evaluación.

Submission conceptual acordada:

```text
ManagementSubmission
    input_id
    resolution_key
    alarm_identity
    source_occurrence_id
    source_evaluated_at
    tool_key
    message_key?
    requested_deactivation_until?
```

`source_occurrence_id` es el target operacional.

`source_evaluated_at` es provenance de auditoría de lo que vio el operador; no es optimistic lock. Una evaluación posterior dentro de la misma occurrence no invalida por sí sola la acción.

El browser no devuelve como autoridad:

```text
evidence payload
cause_text
approval_required
max_duration_hours
resolved Message policy
```

Management Capture valida el exact Effective key, resuelve Message/capability, agrega actor/timestamp confiables y produce el input operacional.

## 22. Engine authority sobre Management result

Target Engine input acordado:

```text
ManagementAction
    input_id
    alarm_identity
    source_occurrence_id
    tool_key
    actor_key
    source_created_at
    deactivation_intent?
        effective_until
        approval_required
```

Aunque Capture haya validado la submission, la occurrence puede cambiar antes del ciclo que aplica la acción. El Engine mantiene autoridad final y produce:

```text
EFFECTIVE
ADDITIONAL
LATE
```

CURRENT ya materializa `ManagementEffect`, `DeactivationRequest`, `DeactivationEffect`, Journey e `InputReceipt`; `input_id` correlaciona el round-trip.

## 23. VERIFIED CURRENT que habilita este diseño

- `AlarmOperationalCycleResult` conserva evaluaciones completas del ciclo;
- `AlarmEvaluation` aporta `EvidenceSnapshot` para ACTIVE/INACTIVE y `EvaluationError` para ERROR;
- `GroupLifecycleDecision` conserva priority, management, deactivation y assignments resueltos;
- open occurrences están en `GroupLifecycleState`;
- pending deactivation requests entran al ciclo como inputs durables;
- Engine decide `EFFECTIVE / ADDITIONAL / LATE`;
- Input receipts correlacionan inputs aplicados con commits.

No existe todavía `EngineResolvedCurrentState`, Live Delivery ni `AlarmLiveProjection` como packages/contratos implementados.

## 24. OPEN

Permanece OPEN:
- owner/package concreto de Live Delivery;
- schema evaluator-evidence para validar `cause_template`;
- proveedor concreto de `shift_end`;
- stale pending-request lifecycle sin decisión;
- physical store/schema/version/codec del Live snapshot;
- publication cadence;
- retention;
- eventual relación visual-target/routing si producto decide una en el futuro.

No inventar estos detalles para implementar los contratos ya cerrados.

## 25. Conflictos con decisions

B.1 frozen Special Cascade sigue en conflicto con la suppression uniforme CURRENT por ranking.

Además, B.1 contiene una formulación donde B.2 exige Message activo, mientras el Project acordó que un Message inactive permanece válido pero no seleccionable para nuevas acciones. Esta reconciliación debe registrarse explícitamente en `atlanticus-decisions`; canonical no la oculta.

## 26. Invariantes congelados

```text
Delivery Configuration y Runtime Configuration comparten AlarmResolutionKey.
Delivery/Live usan sólo el exact Effective key.
Delivery Configuration contiene todas las Rules definidas, incluidas disabled/TRACE_ONLY.
Removed significa ausencia.
Delivery no contiene priority source data ni evaluator code.
Message override se resuelve completamente en B.2.
Inactive Message no es seleccionable para nuevas gestiones.
ToolStructure no se copia al Delivery artifact.
Visual targets y routing assignments siguen siendo contratos distintos.
EngineResolvedCurrentState usa current cycle evaluation, no Evidence History.
Current evidence se usa para materializar cause_text backend-side.
Web no interpreta cause_template.
Technical ERROR no reutiliza un valor físico viejo como current.
Cause materialization failure no oculta una alarma operacional real.
Priority se resuelve antes de Live Delivery.
VISIBLE + PREDOMINANT se publica.
VISIBLE + DEACTIVATED se publica.
ECLIPSED/CASCADE_SUPPRESSED no se publican.
TRACE_ONLY no se publica y no promueve otra Rule.
Management y technical hold son atributos, no motores de priority en Web.
Live snapshot es completo para un resolution_key + as_of; vacío es válido.
Management devuelve identity/intention, no evidence como autoridad.
source_occurrence_id es target operacional.
source_evaluated_at es provenance, no lock.
Engine mantiene autoridad final EFFECTIVE/ADDITIONAL/LATE.
No aliases legacy.
No adapters temporales.
```

## 27. Foco único siguiente

```text
B.2 — Materialization Owner/Package + Implementation Boundary
```

Primero cerrar ownership físico/lógico mínimo; después implementar backend-first en incrementos pequeños y verificables.
