# ADA Command Center — Alarm Configuration Authoring Model

Estado: **CURRENT AUTHORING + B.2 MATERIALIZATION/ADOPTION PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED**

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

Decisions conserva autoridad sobre intención frozen/recorded. Cuando una decisión frozen y `main` difieren, el conflicto permanece explícito.

## 2. Aggregate durable

CURRENT:

```text
AlarmConfiguration
├── rules: tuple[AlarmDefinition, ...]
└── messages: tuple[MessageDefinition, ...]
```

Tool Catalog es externo al payload durable.

```text
LATEST SAVED = LATEST VALID_AT_SAVE
```

Una revisión válida al guardar puede quedar después BLOCKED para materialización si una dependencia externa deriva.

```text
VALID_AT_SAVE != READY_AT_ANY_LATER_TIME != EFFECTIVE
```

## 3. Validation layers

```text
LOCAL VALIDATION
-> tipo
-> forma
-> rango
-> invariantes intrínsecos

B.2 VALIDATION
-> relaciones entre campos
-> relaciones entre Rules
-> referencias externas
-> semántica cross-contract

RUNTIME ADOPTION
-> transición desde el estado operacional EFFECTIVE actual
```

La Web puede restringir inputs, pero no es autoridad final de validación.

Reglas fuertes:

```text
disabled != invalid
B.2 no corrige silenciosamente configuración inválida
```

Una Rule, step, Message o referencia disabled sigue teniendo que ser contractualmente válida.

## 4. Identity y Family

```text
AlarmIdentity(family_key, alarm_key)
```

- `alarm_key`: identidad estable;
- `family_key`: namespace lógico;
- Family != `priority_group`;
- no reintroducir `rule_key`.

## 5. Rule naming

CURRENT distingue:

```text
family_key
alarm_key
rule_name
display_name
title
cause_template
```

## 6. Execution y visibility

### Execution

```text
is_active=false
-> Rule permanece definida
-> no entra al executable target
-> adoption puede cerrar occurrence CONFIGURATION_DISABLED
```

Runtime artifact:

```text
defined identities = active + disabled
PlannedAlarm = sólo active
```

### Visibility

```text
visibility_mode = VISIBLE | TRACE_ONLY
```

PROJECT CONTRACT AGREED:

```text
TRACE_ONLY
-> sigue evaluación/tracing/lifecycle/routing/priority/management
-> Delivery no la publica visiblemente
```

No mapear a `delivery_enabled=false`.

Target:

```text
visibility_mode -> Delivery Configuration only
PlannedAlarm.delivery_enabled -> REMOVE
PriorityDisposition.SHADOW -> REMOVE
```

Una Rule TRACE_ONLY predominante no causa promoción visual de una visible eclipsada.

## 7. Classification

CURRENT:

```text
is_special_condition
kind = RISK | IMPACT
criticality = C1 | C2 | C3
business_category
operational_areas
color
```

Son dimensiones independientes.

## 8. Evaluator y parameters

Authoring:

```text
evaluator_key
parameters: Mapping[str, str | float | bool]
```

Runtime evaluator registry resuelve por `(family_key, evaluator_key)`.

B.2 valida la referencia pero no persiste evaluator callable, `DataRequirements`, `DataLoadPlan` ni `AlarmExecutionSession`.

## 9. Priority

CURRENT:

```text
priority_group
priority_order
```

- positivo;
- único dentro del grupo;
- menor número = mayor prioridad;
- Engine todavía exige IMPACT-before-RISK si ambos existen.

## 10. Management suppression — CLOSED

CURRENT implementado:

```text
managed source priority = P

target.priority_order < P
-> no suppression

target.priority_order > P
-> eligible
```

`kind` no selecciona source/target.

Target elimina filtros históricos de `delivery_enabled`; visibility no altera suppression.

Management suppression no cierra occurrence y no detiene routing.

## 11. Special Conditions

Authoring:

```text
is_special_condition=true
```

Reappearance:

```text
ReappearanceDefinition(
    after_minutes,
    special_conditions: tuple[AlarmIdentity, ...],
)
```

Múltiples referencias usan OR.

B.2 qualification para cada SC:

```text
exists
AND is_special_condition
AND same family
AND same priority_group
AND not self-reference
```

Una SC disabled sigue siendo una referencia válida, aunque no pueda estar ACTIVE mientras no participe de execution.

Engine no transporta `is_special_condition`; recibe identidades calificadas.

Runtime trigger CURRENT está CLOSED: level-triggered, OR, INACTIVE/ERROR no disparan, occurrence cerrada no se resucita, timer+SC produce una única reappearance.

## 12. Reappearance timer materialization

Authoring mantiene minutos:

```text
after_minutes: int | None
```

Target Runtime:

```text
PlannedAlarm.reappearance_after_seconds: int | None
```

Conversión B.2:

```text
minutes * 60
```

Target `ManagementEffect`:

```text
reappearance_due_at: datetime | None
```

`None` permite representar correctamente:
- sólo Special Conditions;
- ningún reappearance automático.

El resolver global CURRENT queda SUPERSEDED como target.

Adoption reconcilia un ManagementEffect abierto desde el `effective_at` original. Si el nuevo due ya pasó, reappearance ocurre en el instante de Adoption.

## 13. B.2 Resolution contract

```text
AlarmResolutionKey
    alarm_configuration_revision
    confirmed_tool_catalog_revision
```

```text
READY
-> Runtime artifact + Delivery artifact
-> same exact key

BLOCKED
-> no operational artifacts
-> EFFECTIVE unchanged
```

Una Rule inválida bloquea toda la revisión.

```text
INVALID != REMOVED
```

## 14. Findings

```text
AlarmResolutionFinding
    code
    severity = BLOCKING | WARNING
    message
    alarm_identity?
    field_path?
    reference_key?
```

## 15. Routing — CONTRACT AGREED

CURRENT Engine:
- C1: origin + destinations inmediatos;
- C2: origin inmediato + delays absolutos desde occurrence start;
- C3: origin only.

B.2 ordena por `step_order`.

### C1

```text
enabled -> immediate
enabled + wait > 0 -> BLOCKING
```

### C2

Todos los steps, incluidos disabled, requieren wait `>=0` y no-None.

Los waits se acumulan sobre todos los steps ordenados; sólo enabled produce destination.

### C3

Cualquier step enabled es BLOCKING.

### Tool references

Validar origin y todos los targets, incluidos disabled.

Mínimo:

```text
exists in Confirmed Tool Catalog
AND reconciliation-GREEN
AND alarm-eligible Tool kind
```

Strategic no es elegible como Alarm Configuration reference.

Restricciones adicionales PROCESS ↔ INTEGRATED_OPERATIONS/tier siguen OPEN.

## 16. Deactivation + Messages — CONTRACT AGREED

Authoring:

```text
AlarmDeactivationDefinition
    enabled
    max_duration_hours
    approval_required
```

Invariantes B.1:

```text
enabled=false
-> max_duration_hours=None
-> approval_required=false

enabled=true
-> max_duration_hours required
-> 1 <= max_duration_hours <= 12
```

Message puede declarar override completo.

Precedencia:

```text
override=None
-> Rule default

override exists
-> replace complete default
```

B.2 materializa:

```text
ResolvedDeactivationPolicy
    enabled
    max_duration_hours
    approval_required
```

por default y por Message.

Message inactive se valida, pero no se ofrece para nuevas gestiones.

Sin Message contextual se usa Rule default.

Seleccionar Message no dispara deactivation automáticamente.

## 17. Management Capture contract

Management Capture valida la intención contra la Delivery Configuration de la resolución EFFECTIVE exacta.

Solicitud conceptual:

```text
alarm_identity
source_occurrence_id
tool_key
resolution_key
message_key?
requested_deactivation_until?
```

No reinterpretar una acción usando una revisión u occurrence posterior.

Si hay deactivation:

```text
effective_until = min(
    requested_until,
    source_created_at + configured_max_duration,
    shift_end,
)
```

Target Engine input:

```text
DeactivationIntent
    effective_until
    approval_required
```

Por tanto:

```text
PlannedAlarm.deactivation_policy -> REMOVE target
```

El origen concreto de `shift_end` sigue OPEN.

## 18. Effective Configuration alignment

PROJECT CONTRACT AGREED:

```text
AlarmEffectiveConfigurationHead
    resolution_key
    effective_at
    adoption_id
```

Delivery y Management Capture usan exactamente:

```text
EffectiveHead.resolution_key
```

Nunca `latest READY`.

Una Adoption exitosa puede avanzar EFFECTIVE aunque no haya mutaciones de hot state, por ejemplo cambios sólo de visibility, Messages o visual metadata.

Runtime Adoption debe clasificar sobre:

```text
source defined identities
UNION
target defined identities
```

incluyendo:

```text
ADDED
ENABLED
```

además de las disposiciones existentes.

Ver `04_ALARM_ENGINE/13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md`.

## 19. Tool/visual references

Alarm Configuration guarda referencias, no duplica Tool Configuration.

Routing y visual projection son contratos distintos.

Strategic visual projection sigue sin inventarse.

Current reconciliation-GREEN todavía necesita un input contractual explícito para B.2.

## 20. Adoption conflicts todavía OPEN de implementación

Mantener visibles:
- criticality mutation CURRENT = structural reset;
- C2 routing mutation CURRENT = compatible;
- C1 routing mutation CURRENT = rejected;
- C3 routing mutation CURRENT = rejected;
- evaluator mutation CURRENT = rejected vs B.1 desired;
- kind mutation CURRENT = rejected vs B.1 desired;
- priority group mutation CURRENT = rejected vs B.1 desired;
- origin Tool semantics conflict;
- timer/SC reconciliation target todavía no implementada.

Una configuración puede ser B.2 READY y posteriormente ser rechazada por Runtime Adoption.

## 21. Provenance cleanup

CURRENT usa:

```text
alarm_configuration_revision
tool_registry_revision
```

Target:

```text
AlarmResolutionKey
```

Occurrence conserva `resolution_key_at_start`; Effective Head conserva la resolución global vigente.

No mantener contrato dual permanente.

## 22. Invariantes congelados

```text
AlarmConfiguration = Rules + Messages.
LATEST SAVED = LATEST VALID_AT_SAVE.
VALID_AT_SAVE != READY_AT_ANY_LATER_TIME != EFFECTIVE.
AlarmIdentity = family_key + alarm_key.
Family != priority_group.
is_active controla execution participation.
disabled != invalid.
TRACE_ONLY sólo controla visibility Delivery.
TRACE_ONLY participa normalmente de priority/Management/routing.
priority_order es autoridad relativa dentro del grupo.
menor priority_order = mayor prioridad.
Management suppression se gobierna por priority_order, no por kind.
Management no cierra physical occurrence.
Management no detiene routing.
Special Condition es explícita en authoring.
Engine no necesita is_special_condition.
Special Condition refs deben calificarse en B.2.
Special Condition trigger usa OR y es level-triggered.
INACTIVE/ERROR no disparan.
Una occurrence cerrada no se resucita.
Timer + SC en mismo ciclo produce una reappearance.
PlannedAlarm es configuración Runtime resuelta, no evaluator code.
PlannedAlarm no es dueño de Message/deactivation authoring policy.
B.2 valida evaluator key; Runtime resuelve callable desplegado.
B.2 READY es atómico para Runtime + Delivery artifacts.
INVALID != REMOVED.
C2 materializa delays acumulados absolutos desde occurrence start.
Steps C2 disabled conservan su intervalo temporal.
Delivery y Management Capture consumen exact Effective resolution key.
EFFECTIVE es global y puede avanzar con cero group hot-state mutations.
No aliases legacy.
No adapters temporales.
```

## 23. Downstream Delivery / Live contract

El cierre B.2 posterior a este authoring model está consolidado en:

```text
16_ALARM_LIVE_DELIVERY_CONTRACT.md
```

Ese contrato conserva `cause_template` como configuración estática, pero Live Delivery materializa `cause_text` usando el `EvidenceSnapshot.payload` actual del Engine. También congela la proyección de Messages/deactivation capability, visual targets resueltos, TRACE_ONLY, exact Effective key y el round-trip de Management.

Authoring no debe absorber esa lógica operacional.

## 24. Foco único siguiente

```text
B.2 — Materialization Owner/Package + Implementation Boundary
```

Primero cerrar ownership físico/lógico mínimo; después implementación incremental backend-first sólo tras consenso.
