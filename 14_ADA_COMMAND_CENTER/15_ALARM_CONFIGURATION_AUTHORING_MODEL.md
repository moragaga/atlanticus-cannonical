# ADA Command Center — Alarm Configuration Authoring Model

Estado: **CURRENT AUTHORING + B.2 PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED**

## 1. Authority checkpoint

Implementación CURRENT auditada:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Canonical base de este delta:

```text
moragaga/atlanticus-cannonical:main
85a12f5ccdf0b5d03992396b13d4b1914b7062a3
```

Decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Decisions conserva autoridad sobre intención frozen/recorded. Cuando una decisión frozen y `main` difieren, el conflicto se mantiene explícito.

## 2. Aggregate durable

CURRENT:

```text
AlarmConfiguration
├── rules: tuple[AlarmDefinition, ...]
└── messages: tuple[MessageDefinition, ...]
```

Tool Catalog es externo al payload durable.

DECISION RECORDED:

```text
LATEST SAVED = LATEST VALID_AT_SAVE
```

La revisión persistida pasó la validación completa exigida en el instante de save.

Una revisión válida al guardar puede quedar después `BLOCKED` para materialización si una dependencia externa deriva.

Por tanto:

```text
VALID_AT_SAVE != READY_AT_ANY_LATER_TIME != EFFECTIVE
```

La implementación CURRENT todavía no materializa B.2 completo; este documento no debe confundir contrato acordado con código ya existente.

## 3. Validation layers

Contrato transversal acordado:

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

La Web puede usar selects, rangos y controles para impedir entradas imposibles, pero la UI no es autoridad de validación.

Backend/B.2 vuelve a validar.

Reglas fuertes:

```text
disabled != invalid
```

Una Rule, step, Message o referencia disabled sigue teniendo que ser contractualmente válida.

```text
B.2 no corrige silenciosamente una configuración inválida
```

Un valor contradictorio se rechaza con finding BLOCKING; no se normaliza hacia otra semántica operacional.

## 4. Identity y Family

```text
AlarmIdentity(family_key, alarm_key)
```

- `alarm_key`: identidad estable.
- `family_key`: namespace lógico.
- Family no equivale a `priority_group`.
- no existe necesidad CURRENT de `FamilyDefinition` durable.

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

No reintroducir aliases legacy retirados.

## 6. Execution y visibility

### Execution

```text
is_active
```

`is_active=false`:
- Rule permanece definida;
- no entra a la execution session target;
- adoption debe poder cerrar occurrence abierta como configuration-disabled.

B.2 acordado:

```text
Runtime artifact:
defined identities = active + disabled
PlannedAlarm = sólo active
```

Esto permite distinguir `DISABLED` de `REMOVED`.

Una Rule disabled sigue siendo validada completamente.

### Visibility

```text
visibility_mode = VISIBLE | TRACE_ONLY
```

`TRACE_ONLY`:
- sigue evaluándose;
- sigue trazándose;
- no debe publicarse visiblemente por Delivery.

No crear `not_visible`.

### Conflict

Engine CURRENT usa:

```text
delivery_enabled=false -> SHADOW
```

y eso afecta priority/Management.

Por tanto:

```text
TRACE_ONLY != delivery_enabled=false
```

B.2 debe reconciliarlo. Sigue OPEN.

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

`is_special_condition`, `kind`, `criticality` y ranking son dimensiones independientes.

## 8. Evaluator y parameters

Authoring:

```text
evaluator_key
parameters: Mapping[str, str | float | bool]
```

Runtime evaluator registry resuelve por:

```text
(family_key, evaluator_key)
```

La lógica Python no vive dentro de Alarm Configuration ni `PlannedAlarm`.

Frontera acordada:

```text
AlarmDefinition
    evaluator_key
    parameters
        |
        v
B.2 validates reference
        |
        v
RuntimeAlarmConfiguration
    PlannedAlarm.evaluator_key
    parameters_by_alarm
        |
        + deployed AlarmEvaluatorRegistry
        v
AlarmExecutionSession
```

B.2 no persiste evaluator callable, `DataRequirements`, `DataLoadPlan` ni `AlarmExecutionSession` como artifact de configuración.

No existe `evaluator_registry_revision` CURRENT; no inventarlo para completar `resolution_key`.

## 9. Priority

CURRENT:

```text
priority_group
priority_order
```

Invariantes:
- positivo;
- único dentro del grupo;
- menor número = mayor prioridad;
- Engine todavía exige IMPACT antes de RISK si ambos existen.

Priority predominance ya usa menor `priority_order`.

## 10. Management suppression — CLOSED

El coupling histórico:

```text
managed IMPACT -> lower RISK
```

fue reemplazado en `main`.

CURRENT:

```text
managed source priority = P

target.priority_order < P
-> no suppression

target.priority_order > P
-> eligible para CascadeSuppression
```

`kind` no selecciona source/target.

`delivery_enabled` histórico todavía participa y queda pendiente de reconciliation con visibility.

Management suppression:
- no cierra occurrence;
- no detiene routing;
- no convierte physical ACTIVE en false;
- puede conservar scope después del cierre de la source occurrence mientras existan lower-priority targets elegibles;
- se libera al terminar su scope/expiry;
- permite que una Rule de mayor prioridad emerja.

Evidencia:
- characterization convertida a regresión;
- 6 PASS;
- full `alarms/core` suite PASS después del delta;
- Ruff PASS.

## 11. Special Conditions

### Authoring

Special Condition es explícita:

```text
is_special_condition=true
```

No se infiere de ranking/kind/criticality.

Reappearance authoring:

```text
ReappearanceDefinition(
    after_minutes,
    special_conditions: tuple[AlarmIdentity, ...],
)
```

Múltiples referencias usan OR.

### Runtime transport — CLOSED

`PlannedAlarm` CURRENT contiene:

```text
reappearance_special_conditions: tuple[AlarmIdentity, ...]
```

Engine no transporta `is_special_condition`.

B.2 materializa sólo referencias calificadas.

### Runtime trigger — CLOSED

CURRENT:

```text
managed A
AND A keeps same open occurrence
AND A evaluated ACTIVE
AND any referenced trigger evaluated ACTIVE
-> reappearance
```

Resultado:
- ManagementEffect cleared;
- misma occurrence;
- `management_cycle + 1`;
- `ReappearanceChange`.

No trigger para:
- unreferenced Rule;
- `INACTIVE`;
- `ERROR`;
- source occurrence cerrada.

Timer + trigger en mismo ciclo produce una única reappearance.

El trigger se decide antes de priority final, desde evaluaciones ACTIVE, por lo que no depende de visibilidad Delivery ni de disposición final de priority.

### Level-trigger semantics — CLOSED

Si la Special Condition referenciada ya está ACTIVE cuando el operador gestiona A:

```text
ManagementAction -> EFFECTIVE
ManagementEffect -> STARTED
ManagementEffect -> CLEARED en el mismo ciclo
same occurrence
management_cycle -> increment
one ReappearanceChange
```

No se requiere detectar un flanco `INACTIVE -> ACTIVE`.

Evidencia final:
- 9 PASS en `test_special_condition_reappearance_characterization.py`;
- full `alarms/core` suite PASS;
- `ruff check .` PASS;
- format check PASS.

## 12. Decisions conflict

B.1 frozen describió Special Cascade:

```text
managed predominant Special Condition
-> suppress all other active Rules in scope
```

El Project refinó e implementó:

```text
Management suppression -> priority_order para cualquier Rule gestionada
is_special_condition -> trigger semantics explícitas
```

Clasificación correcta:

```text
IMPLEMENTATION CURRENT / VALIDATED
PROJECT DECISION / REFINEMENT
DECISIONS REPOSITORY NOT YET UPDATED
CONFLICT VISIBLE
```

No escribir en canonical que la decisión frozen fue formalmente superseded hasta actualizar `atlanticus-decisions`.

## 13. B.2 Resolution contract

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
resolution_key
    alarm_configuration_revision
    confirmed_tool_catalog_revision
```

Resultado:

```text
READY
-> no BLOCKING findings
-> Runtime artifact existe
-> Delivery artifact existe
-> ambos comparten resolution_key

BLOCKED
-> >= 1 BLOCKING finding
-> ningún artifact operacional
-> EFFECTIVE unchanged
```

Una Rule inválida bloquea la revisión completa.

Nunca omitir silenciosamente una Rule inválida del target.

```text
INVALID != REMOVED
```

## 14. Findings

Contrato conceptual acordado:

```text
AlarmResolutionFinding
    code
    severity = BLOCKING | WARNING
    message
    alarm_identity?
    field_path?
    reference_key?
```

`BLOCKING` cubre cualquier inconsistencia que pueda alterar Runtime, adoption, routing, management, deactivation o Delivery correcto.

`WARNING` queda reservado para calidad administrativa no contractual.

B.2 intenta recolectar findings independientes sin generar ruido derivado de una misma dependencia ausente.

## 15. Routing — CONTRACT AGREED

CURRENT Engine:
- C1: origin + destinos inmediatos;
- C2: origin inmediato + destinos retardados desde occurrence start;
- C3: origin only.

Management no detiene routing.

Authoring conserva:

```text
AlarmEscalationDefinition
    origin_tool_key
    steps[]
        step_order
        target_tool_key
        is_enabled
        wait_minutes_from_previous_step
```

### Orden

B.2 ordena steps por `step_order`; el orden físico de la tuple no es autoridad.

No se exige secuencia contigua.

### C1

```text
origin -> inmediato
step enabled -> destination inmediata
step enabled + wait > 0 -> BLOCKING
```

`None` o `0` son coherentes con C1.

### C2

Todos los steps, incluidos disabled, requieren wait no-None y `>= 0`.

Los waits se acumulan sobre todos los steps ordenados.

Sólo enabled steps producen destination.

Ejemplo:

```text
step 1 enabled  15
step 2 disabled 20
step 3 enabled  30
```

materializa:

```text
step 1 -> delay_seconds=900
step 3 -> delay_seconds=3900
```

Deshabilitar un destino no adelanta silenciosamente destinos posteriores.

### C3

```text
origin only
```

Cualquier step enabled es `BLOCKING`.

Steps disabled pueden permanecer, pero sus referencias y shape siguen siendo válidas.

### Tool references

Validar origin y todos los targets, incluidos disabled.

Mínimo:

```text
exists in Confirmed Tool Catalog
AND reconciliation-GREEN
AND alarm-eligible Tool kind
```

Strategic queda fuera de Alarm Configuration references en el contrato acordado.

Las restricciones adicionales PROCESS ↔ INTEGRATED_OPERATIONS/tier siguen OPEN.

## 16. Deactivation — OPEN / NEXT

AlarmDefinition conserva:
- `enabled`;
- `max_duration_hours`;
- `approval_required`.

Messages pueden aplicar override completo.

Engine `PlannedAlarm` conserva actualmente una deactivation policy más estrecha.

La resolución completa default/Message/operator/max/shift es el siguiente subfoco B.2.

No ampliar Engine por conveniencia antes de cerrar el contrato.

## 17. Tool/visual references

Alarm Configuration guarda referencias, no duplica Tool Configuration.

Tool Catalog conserva estructura/scopes.

Routing y visual projection son contratos distintos.

Strategic visual projection sigue sin inventarse.

Current reconciliation-GREEN todavía necesita un input contractual explícito para B.2.

## 18. Adoption conflicts todavía OPEN

Mantener explícitos:
- criticality mutation CURRENT = structural reset;
- C2 routing mutation CURRENT = compatible;
- C1 routing mutation CURRENT = rejected;
- C3 routing mutation CURRENT = rejected;
- evaluator mutation CURRENT = rejected vs B.1 desired;
- kind mutation CURRENT = rejected vs B.1 desired;
- priority group mutation CURRENT = rejected vs B.1 desired;
- origin Tool semantics conflict.

Una configuración puede ser B.2 `READY` y posteriormente ser rechazada por Runtime Adoption.

No resolver con adapters temporales.

## 19. Provenance cleanup

Runtime CURRENT usa:

```text
alarm_configuration_revision
tool_registry_revision
```

B.2 acordó:

```text
alarm_configuration_revision
confirmed_tool_catalog_revision
```

La implementación debe reemplazar limpiamente el naming histórico, sin contrato dual permanente.

## 20. Invariantes congelados

Hasta decisión explícita contraria:

```text
AlarmConfiguration = Rules + Messages.

LATEST SAVED = LATEST VALID_AT_SAVE.

VALID_AT_SAVE != READY_AT_ANY_LATER_TIME != EFFECTIVE.

AlarmIdentity = family_key + alarm_key.

Family != priority_group.

is_active controla execution participation.

disabled != invalid.

TRACE_ONLY controla Delivery visibility y no equivale a Engine SHADOW.

priority_order es la autoridad relativa dentro del grupo.

menor priority_order = mayor prioridad.

IMPACT-before-RISK sigue CURRENT.

Management suppression se gobierna por priority_order, no por kind.

Management no cierra physical occurrence.

Management no detiene routing.

Special Condition es explícita en authoring.

Engine no necesita is_special_condition.

PlannedAlarm transporta reappearance_special_conditions.

Special Condition reappearance usa OR.

Special Condition trigger es level-triggered.

INACTIVE/ERROR no disparan.

Una occurrence cerrada no se resucita.

Timer + Special Condition en el mismo ciclo produce una sola reappearance.

PlannedAlarm es configuración Runtime resuelta, no evaluator code.

B.2 valida evaluator key; Runtime resuelve el callable desplegado.

B.2 READY es atómico para Runtime + Delivery artifacts.

INVALID != REMOVED.

C2 materializa delays acumulados absolutos desde occurrence start.

Steps C2 disabled conservan su intervalo temporal.

B.2 debe calificar/materializar, no reimplementar Engine.

No crear aliases legacy.

No crear adapters temporales.

No modificar Engine por conveniencia de UI/materialización.
```

## 21. Foco único siguiente

```text
B.2 — Deactivation + Messages Materialization Contract
```

Primero debate/diseño.

Después de consenso, continuar con el resto del Runtime/Delivery materialization contract antes de implementación.

No mezclar en el mismo incremento:
- Delivery execution final;
- UI final;
- History/Analytics;
- broad Engine rewrite.
