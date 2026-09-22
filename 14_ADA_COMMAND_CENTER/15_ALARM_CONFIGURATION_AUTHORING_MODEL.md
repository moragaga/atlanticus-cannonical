# ADA Command Center — Alarm Configuration Authoring Model

Estado: **CURRENT / AUTHORITY RECONCILED / MANAGEMENT + SPECIAL-CONDITION RUNTIME CLOSED / B.2 NEXT**

## 1. Authority checkpoint

Implementación CURRENT auditada:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Canonical base previo a esta actualización:

```text
moragaga/atlanticus-cannonical:main
d8b69e914cbdd49c9f53f0c27d90eb38a0c1b86f
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

```text
VALID != FULLY RESOLVED != READY
```

Una referencia Tool/evaluator no resuelta no vuelve intrínsecamente inválida una Source revision.

## 3. Identity y Family

```text
AlarmIdentity(family_key, alarm_key)
```

- `alarm_key`: identidad estable.
- `family_key`: namespace lógico.
- Family no equivale a `priority_group`.
- no existe necesidad CURRENT de `FamilyDefinition` durable.

## 4. Rule naming

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

## 5. Execution y visibility

### Execution

```text
is_active
```

`is_active=false`:
- Rule permanece definida;
- sale de la nueva execution session;
- adoption debe poder cerrar occurrence abierta como configuration-disabled.

No crear `not_execute`.

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

B.2 debe reconciliarlo.

## 6. Classification

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

## 7. Evaluator y parameters

Authoring:

```text
evaluator_key
parameters: Mapping[str, str | float | bool]
```

Runtime evaluator registry resuelve por:

```text
(family_key, evaluator_key)
```

No existe schema genérico por evaluator.

No inventarlo en B.2 ni UI.

## 8. Priority

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

## 9. Management suppression — CLOSED

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

## 10. Special Conditions

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

B.2 deberá materializar y calificar las referencias.

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

## 11. Decisions conflict

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

## 12. Routing

CURRENT:
- C1: origin + destinos configurados inmediatos;
- C2: origin inmediato + destinos retardados;
- C3: origin only.

Management no detiene routing.

Authoring C2 mantiene:

```text
wait_minutes_from_previous_step
```

La conversión propuesta a delays acumulados absolutos sigue OPEN para B.2.

## 13. Deactivation

AlarmDefinition conserva:
- enabled;
- max_duration_hours;
- approval_required.

Engine `PlannedAlarm` conserva deactivation policy de approval.

La resolución completa default/Message/operator/max/shift sigue OPEN para B.2/Delivery.

No ampliar Engine por conveniencia.

## 14. Tool/visual references

Alarm Configuration guarda referencias, no duplica Tool Configuration.

Tool Catalog conserva estructura/scopes.

Routing y visual projection son contratos distintos.

Strategic visual projection sigue sin inventarse.

## 15. Adoption conflicts todavía OPEN

Mantener explícitos:
- criticality mutation CURRENT = structural reset;
- C2 routing mutation CURRENT = compatible;
- C1/C3 routing mutation CURRENT = rejected;
- evaluator mutation CURRENT = rejected vs B.1 desired;
- kind mutation CURRENT = rejected vs B.1 desired;
- priority group mutation CURRENT = rejected vs B.1 desired;
- origin Tool semantics conflict.

No resolver dentro de B.2 sin decisión explícita.

## 16. B.2 — NEXT

La frontera siguiente es:

```text
Alarm Configuration Projection
+ Tool Catalog
+ evaluator/runtime contracts
-> resolved/materialized Runtime input
```

El siguiente chat debe definir primero el contrato de materialización Runtime.

Debe reutilizar, no reinventar, los destinos CURRENT:
- `PlannedAlarm`;
- `AlarmExecutionEntry`;
- evaluator parameters;
- routing;
- deactivation policy;
- `reappearance_special_conditions`;
- adoption.

Debe conservar:

```text
VALID != FULLY RESOLVED != READY
Runtime adoption determines EFFECTIVE
Delivery does not lead Runtime
```

## 17. OPEN para B.2

1. `ResolvedAlarmConfiguration`/equivalente: identidad y provenance.
2. Findings y readiness Runtime.
3. `is_active` → adoption + execution session.
4. `visibility_mode` sin usar `delivery_enabled=false` como alias.
5. evaluator availability + parameters.
6. routing materialization.
7. C2 relative waits → effective delays: contrato aún no frozen.
8. deactivation effective capability.
9. Special Condition qualification → `reappearance_special_conditions`.
10. revision/provenance reconciliation.
11. adoption conflicts enumerados arriba.

## 18. Invariantes congelados

Hasta decisión explícita contraria:

```text
AlarmConfiguration = Rules + Messages.

VALID != FULLY RESOLVED != READY.

AlarmIdentity = family_key + alarm_key.

Family != priority_group.

is_active controla execution participation.

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

B.2 debe calificar/materializar, no reimplementar Engine.

No crear aliases legacy.

No crear adapters temporales.

No modificar Engine por conveniencia de UI/materialización.
```

## 19. Foco único siguiente

```text
B.2 — ALARM CONFIGURATION -> RUNTIME MATERIALIZATION CONTRACT
```

Primero debate/diseño.

Después de consenso, implementación incremental backend.

No mezclar en el mismo incremento:
- Delivery final;
- UI;
- History/Analytics;
- Tool tier routing;
- presentation terminology;
- nuevos cambios del Alarm Engine.
