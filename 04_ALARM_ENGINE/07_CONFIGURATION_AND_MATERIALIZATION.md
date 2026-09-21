# Alarm Engine — Configuration and Materialization

Estado: **CURRENT SEMANTICS / REFINED / IMPLEMENTATION RECONCILIATION OPEN**

## Invariante central

`LATEST SAVED = LATEST VALID`

`VALID` significa aquí **intrínsecamente válida como Alarm Configuration persistible**.

No significa que todas las dependencias externas estén disponibles o resolubles en ese instante.

Una working copy intrínsecamente inválida/incompleta no se convierte en revisión persistida autoritativa.

## Fases

- working copy;
- intrinsic pre-save validation;
- persisted valid Alarm Source revision;
- external resolution/materialization;
- capability readiness;
- Runtime adoption;
- EFFECTIVE.

## Intrinsic pre-save validation

Debe validar el candidate completo en aquello que pertenece a Alarm Configuration, incluyendo según contrato:

- identity/uniqueness;
- priority invariants;
- Special Conditions structure;
- Message/deactivation/reappearance dentro del mismo aggregate;
- escalation structure;
- parameter keys;
- parameter values limitados a `str | float | bool`;
- referencias internas del aggregate.

Un finding intrínseco blocking rechaza save.

## External resolution

Después de persistir una revisión válida, materialization/resolution puede evaluar dependencias externas:

- evaluator disponible;
- Tool disponible en el Tool Catalog;
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

Se permite persistir una Rule que referencia una Tool/evaluator todavía no disponible, siempre que el contrato intrínseco sea válido.

Esa Rule:

- no se considera removed;
- no se considera disabled;
- conserva su historia;
- puede re-resolverse posteriormente sin nueva Alarm Source revision.

## Re-resolution

La identidad/provenance de resolución debe distinguir al menos la Alarm Source revision y la revisión de sus dependencias externas.

```text
Alarm Source A17 + Tool Catalog T40
→ unresolved Tool

Alarm Source A17 + Tool Catalog T41
→ resolved Tool
```

A17 no cambia.

## Drift

Una revisión que fue READY puede dejar de estarlo para una capability por drift externo.

Eso no invalida la historia.

El EFFECTIVE anterior permanece cuando la política de adopción/LKG así lo exige hasta que exista sustituto resoluble/adoptable.

## LKG

Invalid candidate no destruye last-known-good.

`INVALID != REMOVED`.

`UNRESOLVED != INVALID`.

REMOVED debe ser intención explícita.

## Runtime / Delivery

Runtime y Delivery derivan de la misma resolución validada/provenance.

La readiness puede diferir por capability.

Una dependencia exclusivamente visual/routing puede dejar Delivery no READY sin impedir una evaluación Runtime que no necesita esa dependencia.

Delivery no despacha hacia referencias externas no resueltas.

Delivery no puede liderar la configuración EFFECTIVE de Runtime.

## Parameters

Alarm Configuration no incorpora schemas particulares por evaluator.

El contrato genérico permanece:

```text
mapping[str, str | float | bool]
```

Los nombres y semántica de parameters pertenecen al evaluator/desarrollador.

Errores de uso deben resultar observables en resolución/ejecución conforme al contrato del Runtime, no generar una familia de modelos aislados en Alarm Configuration.

## Storage

Alarm Configuration adopta Source/Release CURRENT de Atlanticus y Blob como provider durable objetivo en dominios migrados.

El Tool Catalog reconciliado también tiene Blob como durable target, pero es estado derivado con lifecycle/revisión independiente de Alarm Source.

No duplicar Tool topology en Cosmos de Command Center sin una necesidad de consumo demostrada.
