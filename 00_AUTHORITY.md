# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

- Repositorio: `moragaga/atlanticus`
- Rama: `main`
- Realidad implementada: siempre `atlanticus:main`
- Checkpoint CURRENT verificado para este cierre:
  `9398786ae9af7c00de1bcca9d7a311fe9ef2155f`
- Parent inmediato:
  `b9d0085387388b662bbaf7e76030b7a983b8c76e`
- Fecha observada del commit:
  `2026-09-22T23:26:38Z`

El commit `9398786ae9af7c00de1bcca9d7a311fe9ef2155f` contiene el incremento relevante de este cierre:
- `resolver.py` productivo B.2;
- export público `resolve_alarm_configuration`;
- espejo pedagógico comentado;
- `tests/test_resolver.py`.

Los commits intermedios posteriores al checkpoint Alarm anterior que sólo tocaron tooling u otros scopes no
cambian los contratos de Alarm Engine y no forman parte de este cierre semántico.

Estado acumulado relevante para ADA Command Center Alarm Engine:

```text
COMMAND-CENTER-ALARM-DOMAIN-EXTRACTION              CLOSED / VERIFIED / CURRENT
ALARM-CORE-RUNTIME-VISIBILITY-ROOT-REMOVAL         CLOSED / VERIFIED / CURRENT
B.2-MATERIALIZATION-CONTRACTS                      CLOSED / VERIFIED / CURRENT
B.2-RESOLVER-QUALIFICATION-INPUT-CONTRACTS         CLOSED / VERIFIED / CURRENT
DEACTIVATION-CASCADE-SCOPE                         CLOSED / VERIFIED / CURRENT
RUNTIME-REAPPEARANCE-AFTER-SECONDS-CONTRACT        CLOSED / VERIFIED / CURRENT
PURE-B.2-RESOLVER                                  CURRENT / IMPLEMENTED
B.2-MATERIALIZATION-PROCESS                        PLANNED / NEXT
RUNTIME-ADOPTION-EFFECTIVE-HEAD                    PLANNED
ALARM-LIVE-DELIVERY                                PLANNED
```

Qualification observada durante el cierre del resolver:

```text
uv run pytest
31 passed

uv run ruff check .
All checks passed
```

El último `ruff format --check .` compartido antes del commit todavía pedía reformatear los dos
`resolver.py`. El commit CURRENT contiene blobs posteriores distintos de aquel ZIP previo, pero este
cierre no dispone de una salida explícita posterior de `ruff format --check .`.

Clasificación:

```text
resolver implementation on main       VERIFIED / CURRENT
31 tests                               VERIFIED
ruff check                             VERIFIED
final ruff format --check evidence     UNVERIFIED
```

No inferir el último gate sólo por la existencia del commit. Confirmarlo como gate de entrada del
siguiente incremento; si está GREEN, no reabrir diseño ni comportamiento del resolver.

### Canonical

- Repositorio: `moragaga/atlanticus-cannonical`
- Rama: `main`
- Checkpoint inspeccionado antes de este reemplazo:
  `7fea2819aa4c22f9d7494cbe79ab8740ee3f4366`

`atlanticus-cannonical:main` es autoridad documental vigente, subordinada a
`atlanticus:main` cuando la implementación publicada demuestra un estado posterior.

### Historical decisions

- Repositorio: `moragaga/atlanticus-decisions`
- Rama: `main`
- Checkpoint observado:
  `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`

Permanece **HISTORICAL**.

Las decisiones B.1/B.2 preservan intención contractual útil, pero no prevalecen sobre implementación
CURRENT ni sobre refinamientos explícitos posteriores del Project.

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

## B.2 Materialization CURRENT

Package:

```text
scopes/ada-command-center/backend/alarms/materialization
ada-command-center-alarms-materialization==1.0.0
ada_command_center.alarms.materialization
```

Punto de entrada CURRENT:

```python
resolve_alarm_configuration(...)
```

Frontera:

```text
AlarmConfiguration
+ alarm_configuration_revision
+ Confirmed Tool Catalog compatible con revision + get(tool_key)
+ ToolReconciliationQualification
+ EvaluatorQualificationCatalog
        |
        v
pure deterministic resolver
        |
        v
AlarmConfigurationResolution
```

No I/O, stores, scheduler, acquisition ni orchestration.

Atomicidad:

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

El resolver construye `AlarmResolutionKey` desde la revisión Alarm authored y la revisión exacta del
Confirmed Tool Catalog; no recibe una segunda revisión Tool independiente.

## Resolver CURRENT — invariantes congeladas

- candidate completo; no publication parcial por Rule;
- evaluator qualification para toda Rule definida, incluso disabled;
- todas las referencias Tool definidas deben existir en el catálogo exacto y estar GREEN;
- los escalation steps disabled no son routing ejecutable, pero su referencia Tool sigue siendo
  parte de la definición y se califica;
- C1: enabled steps sólo admiten `None | 0` y materializan destinos inmediatos;
- C2: cada enabled step exige wait `> 0`; las esperas relativas se acumulan y se materializan como
  offsets absolutos `delay_seconds` desde el inicio de la occurrence;
- steps disabled no contribuyen al acumulado C2;
- C3: no admite enabled escalation steps y Runtime queda sólo con origin;
- active Rule -> defined + PlannedAlarm + parameters;
- disabled Rule -> defined + Delivery, sin PlannedAlarm ni parameters Runtime;
- inactive Message es válido pero no se materializa como opción para nueva gestión;
- Message `deactivation_override` reemplaza completamente el default;
- `ReappearanceDefinition.after_minutes`: `None -> None`, `M -> M * 60`;
- Process visual target requiere `process_projection_mode`;
- Integrated Operations visual target no admite `process_projection_mode`;
- Strategic visual target queda BLOCKED mientras la proyección Alarm siga indefinida;
- visual components/subcomponents deben existir en `ToolStructure`;
- Delivery materializa referencias estables y no copia `ToolStructure`;
- findings CURRENT usados por el resolver:
  `evaluator_not_qualified`,
  `tool_reference_not_found`,
  `tool_reference_not_green`,
  `routing_invalid_for_criticality`,
  `visual_target_invalid`;
- findings se producen en orden determinístico;
- validaciones internas ya garantizadas por `AlarmConfiguration` no se duplican en el resolver;
- malformed DTO/input contract sigue siendo error de programación, no finding B.2.

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

## Conflictos visibles

Project baseline:

```text
Python 3.14.7
```

ADA Command Center backend CURRENT:

```text
requires-python ==3.14.2
```

Permanece OPEN y separado de Materialization salvo bloqueo demostrado.

`atlanticus-decisions` conserva formulaciones históricas incompatibles o menos precisas que CURRENT:
- Special Cascade B.1 vs suppression uniforme por `priority_order`;
- B.1 no expresa deactivation vigente como fuente independiente de cascade suppression;
- Message activo requerido históricamente vs inactive Message válido pero no seleccionable;
- Strategic visual behavior quedó históricamente sin contrato específico; CURRENT B.2 bloquea
  Strategic visual targets;
- el detalle C2 relativo -> offset acumulado queda refinado por implementación CURRENT.

## Siguiente foco único

```text
B.2 MATERIALIZATION PROCESS
PLANNED / NEXT
```

Gate de entrada:
- confirmar una vez `uv run ruff format --check .` sobre el checkpoint CURRENT;
- si GREEN, no reabrir el resolver.

El siguiente incremento debe diseñar primero la frontera del proceso que:
- adquiere los inputs;
- compara revisions cuando corresponda;
- invoca el resolver puro;
- persiste/publica artifacts y findings según contratos explícitos;
- emite diagnósticos operacionales.

No mezclar con Runtime Adoption, Live Delivery, Management Capture ni broad Core cleanup.
