# Alarm Engine — Configuration and Materialization

Estado: **CURRENT SEMANTICS / ALARM SOURCE + BASE PROJECTION IMPLEMENTED / B.2 OPEN**

## Invariante central

`LATEST SAVED = LATEST VALID`

`VALID` significa aquí **intrínsecamente válida como Alarm Configuration persistible**.

No significa que todas las dependencias externas estén disponibles o resolubles en ese instante.

Una working copy intrínsecamente inválida/incompleta no se convierte en revisión persistida
autoritativa.

## Implementación CURRENT previa a B.2

Bajo:

```text
scopes/ada-command-center/web/alarms/configuration
```

ya existen:

```text
AlarmConfiguration aggregate
→ full-revision intrinsic validation
→ Source/Release
→ exact base Projection[AlarmConfiguration]
→ Manager workflow/history
```

La base Projection no resuelve dependencias externas y tiene:

```text
ProjectionTarget.dependencies == ()
```

No confundirla con `ResolvedAlarmConfiguration`.

## Fases

Estado actual:

- working copy — **CURRENT mediante Manager workspace**;
- intrinsic pre-save validation — **CURRENT**;
- persisted valid Alarm Source revision — **CURRENT contract**;
- base Alarm Configuration Projection — **CURRENT**;
- external resolution/materialization — **PLANNED / B.2**;
- capability readiness — **PLANNED / B.2**;
- Runtime adoption — **EXISTING RUNTIME / RECONCILIATION OPEN**;
- EFFECTIVE — **RECONCILIATION OPEN**.

## Intrinsic pre-save validation CURRENT

Valida el candidate completo en aquello que pertenece a Alarm Configuration, incluyendo:

- identity/uniqueness;
- `rule_name` uniqueness within family;
- priority invariants;
- Special Condition references dentro del aggregate;
- Message references dentro del aggregate;
- deactivation/reappearance structure;
- escalation structure;
- parameter keys;
- parameter values limitados a `str | float | bool`.

CURRENT implementation también fija:

- `message_key` único dentro del aggregate;
- una referencia a Message inactivo sigue siendo intrínsecamente válida.

La disponibilidad efectiva de contenido para Delivery se resolverá posteriormente y no invalida
retrospectivamente la Source revision.

Un finding intrínseco blocking rechaza save/publication del aggregate.

## External resolution PLANNED

Después de persistir/proyectar una revisión válida, B.2 podrá evaluar dependencias externas:

- evaluator disponible;
- Tool disponible en el Command Center Tool Catalog;
- Component/Subcomponent resoluble;
- Tool type/projection mode;
- visual targets;
- routing/escalation targets;
- external topology drift.

Un finding externo no convierte retrospectivamente la Alarm Source revision en inválida.

Puede impedir que una capability quede READY.

```text
VALID
!=
FULLY RESOLVED
!=
READY FOR EVERY CAPABILITY
```

## Preconfiguration

Se permite persistir una Rule que referencia una Tool/evaluator todavía no disponible, siempre que
el contrato intrínseco sea válido.

Esa Rule:

- no se considera removed;
- no se considera disabled;
- conserva su historia;
- puede re-resolverse posteriormente sin nueva Alarm Source revision.

## Re-resolution

La identidad/provenance de resolución debe distinguir al menos la Alarm Source revision y la revisión
de sus dependencias externas.

```text
Alarm Source A17 + Tool Catalog T40
→ unresolved Tool

Alarm Source A17 + Tool Catalog T41
→ resolved Tool
```

A17 no cambia.

El contrato exacto de Tool Catalog es el siguiente prerequisite antes de B.2.

## Drift

Una revisión que fue READY puede dejar de estarlo para una capability por drift externo.

Eso no invalida la historia.

El EFFECTIVE anterior permanece cuando la política de adopción/LKG así lo exige hasta que exista
sustituto resoluble/adoptable.

## LKG

Invalid candidate no destruye last-known-good.

```text
INVALID != REMOVED
UNRESOLVED != INVALID
```

REMOVED debe ser intención explícita.

## Runtime / Delivery

Runtime y Delivery derivarán de la misma resolución validada/provenance.

La readiness puede diferir por capability.

Una dependencia exclusivamente visual/routing puede dejar Delivery no READY sin impedir una
evaluación Runtime que no necesita esa dependencia.

Delivery no despacha hacia referencias externas no resueltas.

Delivery no puede liderar la configuración EFFECTIVE de Runtime.

No existe todavía implementación B.2 ni `ResolvedAlarmConfiguration` en `main` al checkpoint de este
cierre.

## Runtime CURRENT a reconciliar

El runtime existente conserva revision strings históricos:

```text
alarm_configuration_revision
tool_registry_revision
```

La reconciliación futura debe reemplazar limpiamente ese provenance donde corresponda. No introducir
adapters temporales ni doble contrato.

## Parameters

Alarm Configuration no incorpora schemas particulares por evaluator.

El contrato genérico permanece:

```text
mapping[str, str | float | bool]
```

Los nombres y semántica de parameters pertenecen al evaluator/desarrollador.

Errores de uso deben resultar observables en resolución/ejecución conforme al contrato del Runtime,
no generar una familia de modelos aislados en Alarm Configuration.

## Storage

Alarm Configuration adopta Source/Release CURRENT de Atlanticus y Blob como provider durable objetivo
en dominios migrados.

El package CURRENT recibe `SourceStore` explícitamente; el binding productivo Blob de la aplicación
Command Center permanece OPEN.

El Command Center Tool Catalog también tiene Blob como durable target de dirección, pero es estado
derivado/reconciliado con lifecycle/revisión independiente de Alarm Source.

No duplicar Tool topology en Cosmos de Command Center sin una necesidad de consumo demostrada.
