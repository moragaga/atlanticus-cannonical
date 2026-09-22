# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `bc3fffd72afb712d5b5ab84522c379abf2a19642`
- Parent inmediato:
  `345309c07d4489a5c477f0fe61620faa91dfe9eb`
- Fecha observada del commit:
  `2026-09-22T20:18:48Z`

Estado acumulado relevante para ADA Command Center Alarm Engine:

```text
COMMAND-CENTER-ALARM-DOMAIN-EXTRACTION          CLOSED / VERIFIED / CURRENT
ALARM-CORE-RUNTIME-VISIBILITY-ROOT-REMOVAL     CLOSED / VERIFIED / CURRENT
B.2-MATERIALIZATION-CONTRACTS                  CLOSED / VERIFIED / CURRENT
B.2-RESOLVER-QUALIFICATION-INPUT-CONTRACTS     CLOSED / VERIFIED / CURRENT
PURE-B.2-RESOLVER                              PLANNED / NEXT
B.2-MATERIALIZATION-PROCESS                    PLANNED
RUNTIME-ADOPTION-EFFECTIVE-HEAD                PLANNED
ALARM-LIVE-DELIVERY                            PLANNED
```

Los cambios entre `345309c...` y `bc3fffd72afb712d5b5ab84522c379abf2a19642` pertenecen únicamente al incremento
B.2 Resolver Qualification Inputs bajo `scopes/ada-command-center/backend/alarms/materialization`.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `2d8cbc33b7776e057e4f7d82def318d5eaf8f336`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

### Historical decisions

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- Checkpoint observado:
  `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`

Permanece **HISTORICAL**.

Las decisiones B.1/B.2 preservan intención contractual útil, pero no prevalecen sobre
implementación CURRENT cuando describen contratos ya refinados o reemplazados por el Project.

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

El root removal de Runtime visibility está CLOSED:

```text
PlannedAlarm.delivery_enabled
REMOVED

PriorityDisposition.SHADOW
REMOVED
```

Visibility `VISIBLE | TRACE_ONLY` pertenece al authored Domain y a Delivery, no al Runtime Core.

`AlarmResolutionKey` está implementado en Core como VO operacional compartido:

```text
AlarmResolutionKey
    alarm_configuration_revision
    confirmed_tool_catalog_revision
```

No agregar evaluator revision a ese key sin contrato explícito.

## B.2 Materialization CURRENT

Package:

```text
scopes/ada-command-center/backend/alarms/materialization
ada-command-center-alarms-materialization==1.0.0
ada_command_center.alarms.materialization
```

Contratos CURRENT:
- `RuntimeAlarmConfiguration`;
- `DeliveryAlarmConfiguration`;
- `ResolvedDeliveryAlarm`;
- `ResolvedDeliveryMessage`;
- `ResolvedDeactivationPolicy`;
- resolved visual target VOs;
- `AlarmResolutionStatus`;
- `AlarmResolutionFindingSeverity`;
- `AlarmResolutionFinding`;
- `AlarmConfigurationResolution`.

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

No existe readiness parcial por Rule ni por artifact.

## B.2 Qualification Inputs CURRENT

Materialization expone contratos mínimos:

```text
ToolReconciliationQualification
    green_tool_keys
    is_green(tool_key)

EvaluatorQualificationKey
    family_key
    evaluator_key

EvaluatorQualificationCatalog
    qualified_keys
    is_qualified(family_key, evaluator_key)
```

Estos contratos no crean taxonomía RED/DRIFT/MISSING y no transportan evaluator callables,
`DataRequirement`, `DataLoadPlan` ni Runtime registry.

Un Tool ausente de `green_tool_keys` significa solamente que no está GREEN para B.2.

La producción/adquisición concreta de esas qualifications sigue fuera de estos contratos.

## Conflictos visibles

Project baseline:

```text
Python 3.14.7
```

Packages Command Center CURRENT:

```text
requires-python ==3.14.2
```

Permanece OPEN.

`atlanticus-decisions` conserva formulaciones históricas incompatibles con CURRENT:
- Special Cascade B.1 vs suppression uniforme por `priority_order`;
- Message activo requerido vs inactive Message válido pero no seleccionable.

No resolver silenciosamente.

## Siguiente foco único

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
PLANNED / NEXT
```

Debe ser puro y consumir inputs explícitos.

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
- broad Engine cleanup.
