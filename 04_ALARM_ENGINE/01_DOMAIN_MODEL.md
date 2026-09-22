# Alarm Engine — Domain Model

Estado: **CURRENT / IMPLEMENTED IN CORE / B.2 BOUNDARY REFINED / B.1 DECISIONS CONFLICT STILL VISIBLE**

Fuentes de intención:
- `R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md`;
- `R3.6M-006B.2-alarm-projection-boundary-DECISION-RECORDED.md`;
- `R3.6M-006B.2-alarm-projection-and-publication-boundary-DECISION-RECORDED-INCREMENT-2.md`;
- refinamientos explícitos del Project implementados y validados en `atlanticus:main`;
- contrato B.2 acordado en Project y todavía no implementado.

Realidad implementada auditada:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

## Ownership

Alarm Engine core no conoce:
- Cosmos;
- SharePoint;
- Key Vault;
- Dash/Flask;
- geometría UI;
- sesiones web;
- WAL/leases como contrato de dominio;
- Alarm Configuration editable;
- Tool Catalog como source de authoring.

Recibe contratos runtime ya resueltos/materializados.

## Conceptos

- `AlarmDefinition`: definición editable canónica de una Rule.
- `PlannedAlarm`: política Runtime resuelta de una Rule ejecutable.
- `AlarmEvaluatorContract`: lógica Python desplegada + `DataRequirements`, resuelta por `(family_key, evaluator_key)`.
- `AlarmExecutionEntry`: unión Runtime de `PlannedAlarm + evaluator contract + parameters`.
- `AlarmEvaluation`: resultado físico/técnico del ciclo.
- `Occurrence`: activación de una Rule.
- `Episode`: lifecycle compartido por `priority_group`.
- `ManagementEffect`: efecto temporal de una acción de Management.
- `CascadeSuppression`: supresión operacional derivada de un ManagementEffect.
- `ReappearanceChange`: reaparición operacional de la misma occurrence gestionada.

## Identidad

```text
AlarmIdentity(family_key, alarm_key)
```

`alarm_key` es estable.

No introducir `rule_key` paralelo.

Family y `priority_group` son conceptos distintos.

## AlarmDefinition, PlannedAlarm y evaluator

La frontera acordada es:

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

`PlannedAlarm` **no contiene la lógica Python que evalúa la condición**.

La implementación de la condición pertenece al código desplegado mediante `AlarmEvaluatorContract`.

B.2 puede validar que `(family_key, evaluator_key)` exista, pero no serializa ni transporta callables, `DataRequirements`, `DataLoadPlan` ni `AlarmExecutionSession` como artifact de configuración.

Runtime vuelve a resolver el evaluator desplegado antes de construir la execution session.

## PlannedAlarm CURRENT

`PlannedAlarm` implementado contiene, entre otros:

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

`reappearance_special_conditions` es:

```text
tuple[AlarmIdentity, ...]
```

y no admite duplicados.

El Engine no necesita transportar `is_special_condition`; recibe únicamente las identidades ya calificadas como triggers válidos. La qualification corresponde a B.2/Alarm Configuration.

## Runtime provenance — deuda de naming

CURRENT todavía usa el nombre histórico:

```text
tool_registry_revision
```

La autoridad vigente externa es el `Confirmed Tool Catalog` / `ToolCatalogSnapshot.revision`.

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
tool_registry_revision
-> tool_catalog_revision / confirmed_tool_catalog_revision semantics
```

La implementación debe hacer un reemplazo limpio, sin alias permanentes ni doble provenance paralela.

## Priority

Invariantes CURRENT:

```text
priority_order > 0
priority_order único dentro del priority_group
menor priority_order = mayor prioridad
```

El Engine todavía valida:

```text
todos los IMPACT preceden a todos los RISK
```

cuando ambos kinds existen en el mismo grupo.

Ese invariante no fue modificado en este hito.

## Management suppression CURRENT

La supresión ya no depende de:

```text
source.kind == IMPACT
target.kind == RISK
```

La regla implementada es:

```text
managed source priority = P

target.priority_order < P
-> no suppressed por esa fuente

target.priority_order == P
-> source

target.priority_order > P
-> eligible para CascadeSuppression mientras el ManagementEffect conserve alcance
```

La elegibilidad CURRENT todavía usa `delivery_enabled`; no confundir esta propiedad histórica con `visibility_mode=TRACE_ONLY`.

Management suppression:
- no cierra la occurrence target;
- no cambia su condición física;
- no detiene routing;
- se libera cuando el ManagementEffect deja de tener alcance;
- permite que una Rule de mayor prioridad emerja sobre una gestión de menor prioridad.

El `kind` permanece como clasificación de negocio, no como selector de suppression.

## Special Condition

En Alarm Configuration, Special Condition sigue siendo una Rule normal marcada explícitamente:

```text
is_special_condition=true
```

No se deriva de:
- ranking;
- kind;
- criticality.

En Runtime, `PlannedAlarm` no necesita el flag; consume referencias calificadas en:

```text
reappearance_special_conditions
```

## Reappearance CURRENT

Reappearance temporal existente:

```text
ManagementEffect.reappearance_due_at
```

Cuando corresponde y la occurrence principal sigue vigente:
- se limpia el `ManagementEffect`;
- se conserva la misma occurrence;
- incrementa `management_cycle`;
- se emite `ReappearanceChange`.

### Reappearance por Special Condition

CURRENT e implementado:

```text
managed Rule A
AND A sigue evaluada ACTIVE
AND A conserva la misma occurrence del ManagementEffect
AND alguna identidad en A.reappearance_special_conditions está evaluada ACTIVE
-> A reappears
```

Semántica OR:

```text
SC1 OR SC2 OR SC3
```

Una Rule no referenciada no dispara el efecto.

`INACTIVE` y `ERROR` no disparan.

Una occurrence principal cerrada no se resucita.

El trigger es **level-triggered**: si una Special Condition referenciada ya está ACTIVE cuando se gestiona A, la acción de Management puede registrarse como `EFFECTIVE`, pero su ManagementEffect se crea y se limpia dentro del mismo ciclo; se mantiene la misma occurrence, `management_cycle` incrementa y se emite una única reappearance.

El trigger se resuelve desde evaluaciones ACTIVE antes del cálculo final de priority, por lo que no depende de la disposición final de prioridad ni de visibilidad Delivery.

Si timer y Special Condition coinciden en el mismo ciclo, sólo se produce una reappearance.

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
