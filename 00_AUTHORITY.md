# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `cd08bd8d2c25bd89eb39fa15cbda209c8e9be617`
- Parent inmediato:
  `431384326890d4d0b84d977a980e325a969c0e42`
- Fecha observada del commit:
  `2026-09-22T21:53:31Z`

Estado acumulado relevante para ADA Command Center Alarm Engine:

```text
COMMAND-CENTER-ALARM-DOMAIN-EXTRACTION              CLOSED / VERIFIED / CURRENT
ALARM-CORE-RUNTIME-VISIBILITY-ROOT-REMOVAL         CLOSED / VERIFIED / CURRENT
B.2-MATERIALIZATION-CONTRACTS                      CLOSED / VERIFIED / CURRENT
B.2-RESOLVER-QUALIFICATION-INPUT-CONTRACTS         CLOSED / VERIFIED / CURRENT
DEACTIVATION-CASCADE-SCOPE                         CLOSED / VERIFIED / CURRENT
RUNTIME-REAPPEARANCE-AFTER-SECONDS-CONTRACT        CLOSED / VERIFIED / CURRENT
PURE-B.2-RESOLVER                                  PLANNED / NEXT
B.2-MATERIALIZATION-PROCESS                        PLANNED
RUNTIME-ADOPTION-EFFECTIVE-HEAD                    PLANNED
ALARM-LIVE-DELIVERY                                PLANNED
```

Desde el checkpoint documental anterior `bc3fffd72afb712d5b5ab84522c379abf2a19642`,
los cambios Alarm Core de este cierre están contenidos en:

```text
d48abf17689e7dd8ef93827415b71b7b6be4385b
    deactivation cascade scope

cd08bd8d2c25bd89eb39fa15cbda209c8e9be617
    PlannedAlarm.reappearance_after_seconds
```

Otros commits intermedios tocaron otros frentes y no forman parte de este cierre semántico.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `56943d94889719544f426322ded4a877245dfaee`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

### Historical decisions

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- Checkpoint observado:
  `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`

Permanece **HISTORICAL**.

Las decisiones B.1/B.2 preservan intención contractual útil, pero no prevalecen sobre
implementación CURRENT ni sobre refinamientos explícitos posteriores del Project.

## Jerarquía

1. `atlanticus:main`: realidad implementada.
2. `atlanticus-cannonical:main`: contratos, fronteras, roadmap y estado vigente.
3. Qualification/tests vigentes: evidencia de propiedades demostradas.
4. Decisiones explícitas del Project todavía no formalizadas en canonical: delta temporal.
5. `atlanticus-decisions`: referencia histórica.
6. Memoria/historial conversacional: pista, nunca autoridad suficiente.

## Clasificación obligatoria

```text
VERIFIED
INFERRED
ASSUMED
PROPOSED
UNVERIFIED
```

Estados:

```text
CURRENT
IN PROGRESS
PLANNED
SUPERSEDED
BLOCKED
CLOSED
```

Si implementación y canonical se contradicen, exponer el conflicto y actualizar canonical;
nunca retroceder implementación CURRENT para satisfacer documentación obsoleta.

## Git

Git es **READ ONLY** por defecto.

No crear commits, push, ramas, PR, issues ni mutaciones remotas sin autorización explícita.

## Continuidad congelada

No reabrir sin conflicto demostrado:

```text
ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN

OLD SCHEMA RUNTIME READERS
FORBIDDEN
```

## ADA Command Center Alarm Domain CURRENT

Authority authored:

```text
scopes/ada-command-center/domain/alarms
ada_command_center.domain.alarms
```

Owner de:
- `AlarmIdentity`;
- `AlarmKind`;
- `Criticality`;
- authoring definitions;
- `AlarmConfiguration`;
- validación pura del aggregate.

Web y Backend consumen este dominio. Domain no depende de Web, Runtime, Persistence ni infraestructura.

## Alarm Core CURRENT

Backend owner:

```text
scopes/ada-command-center/backend/alarms/core
ada_command_center.alarms.core
```

Runtime visibility cleanup CLOSED:

```text
PlannedAlarm.delivery_enabled
REMOVED

PriorityDisposition.SHADOW
REMOVED
```

Visibility `VISIBLE | TRACE_ONLY` pertenece al authored Domain y a Delivery, no al Runtime Core.

`AlarmResolutionKey` CURRENT:

```text
AlarmResolutionKey
    alarm_configuration_revision
    confirmed_tool_catalog_revision
```

No agregar evaluator revision sin contrato explícito.

`PlannedAlarm` CURRENT incluye:

```text
deactivation_policy
reappearance_after_seconds
reappearance_special_conditions
```

`reappearance_after_seconds` es `None | int > 0`.

## Deactivation cascade CURRENT

Una deactivation efectiva es una fuente independiente de cascade suppression.

Mientras `DeactivationEffect` esté vigente:

```text
source -> DEACTIVATED
active targets del mismo priority_group con menor prioridad
(priority_order numéricamente mayor)
-> CASCADE_SUPPRESSED
```

Una liberación temporal o por Special Condition del `ManagementEffect` no atraviesa una
deactivation vigente.

`CascadeSuppression` identifica exactamente una causa:

```text
management_effect_id XOR deactivation_effect_id
```

Si ambos efectos están vigentes, deactivation domina la atribución causal de la suppression.

Pending approval no suprime hasta materializar un `DeactivationEffect`.

Routing continúa durante deactivation/suppression.

## B.2 Materialization CURRENT

Package:

```text
scopes/ada-command-center/backend/alarms/materialization
ada-command-center-alarms-materialization==1.0.0
ada_command_center.alarms.materialization
```

Atomicidad congelada:

```text
READY
=> no BLOCKING
=> Runtime existe
=> Delivery existe
=> ambos usan la misma AlarmResolutionKey

BLOCKED
=> existe BLOCKING
=> Runtime is None
=> Delivery is None
```

Qualification inputs CURRENT:

```text
ToolReconciliationQualification
EvaluatorQualificationKey
EvaluatorQualificationCatalog
```

La adquisición concreta sigue fuera del resolver puro.

## Conflictos visibles

Project baseline:

```text
Python 3.14.7
```

ADA Command Center backend CURRENT:

```text
requires-python ==3.14.2
```

Permanece OPEN y separado del resolver.

`atlanticus-decisions` conserva formulaciones históricas incompatibles o menos precisas que CURRENT:
- Special Cascade B.1 vs suppression uniforme por `priority_order`;
- B.1 no expresa la deactivation vigente como fuente independiente de cascade suppression;
- Message activo requerido en formulación histórica vs inactive Message válido pero no seleccionable.

## Siguiente foco único

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
PLANNED / NEXT
```

Debe ser puro y consumir inputs explícitos.

Entre sus materializaciones Runtime deberá resolver:

```text
ReappearanceDefinition.after_minutes
    None -> PlannedAlarm.reappearance_after_seconds = None
    M    -> PlannedAlarm.reappearance_after_seconds = M * 60
```

No mezclar con:
- acquisition/I/O;
- stores;
- scheduler;
- `backend/processes/alarms-materialization`;
- Runtime Adoption;
- Effective Head;
- Live Delivery;
- Management Capture;
- provenance migration;
- deactivation ownership cleanup;
- broad Engine cleanup.
