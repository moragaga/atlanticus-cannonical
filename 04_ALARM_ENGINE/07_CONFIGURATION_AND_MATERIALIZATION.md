# Alarm Engine — Configuration and Materialization

Estado: **CURRENT SEMANTICS / STORAGE RECONCILIATION OPEN**

## Invariante central

`LATEST SAVED = LATEST VALID`

Increment 2 endureció el modelo: una working copy inválida/incompleta no se convierte en revisión persistida autoritativa.

## Fases

- working copy
- pre-save validation
- persisted valid revision
- materialization/revalidation
- READY
- Runtime adoption
- EFFECTIVE

## Pre-save

Debe validar el candidate completo, incluyendo según contrato:

- uniqueness;
- priority;
- Special Conditions;
- message/deactivation;
- evaluator;
- Tool confirmado;
- reconciliation;
- component/subcomponent;
- visual targets;
- process projection;
- escalation/routing.

Blocking finding rechaza save.

Warning puede quedar como quality/admin signal según política todavía no totalmente congelada.

## Drift

Una revisión que fue válida al persistir puede dejar de estar READY por drift externo.

Eso no invalida la historia: el EFFECTIVE anterior permanece hasta que exista sustituto válido/adoptado.

## LKG

Invalid candidate no destruye last-known-good.

`INVALID != REMOVED`.

REMOVED debe ser intención explícita.

## Runtime / Delivery

Ambos derivan de la misma resolución validada.

Delivery no puede liderar a Runtime.

## Blob migration

Las reglas anteriores son semánticas. El storage histórico SharePoint se considera pendiente de sustitución por Blob en dominios migrados. No inventar aún esquema físico nuevo.
