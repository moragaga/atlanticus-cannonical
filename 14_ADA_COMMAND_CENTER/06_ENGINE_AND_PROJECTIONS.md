# ADA Command Center — Engine and Projections

Estado: **CURRENT ENGINE / B.2 RESOLUTION BOUNDARY REFINED / DELIVERY EXECUTION STILL PLANNED**

## Alarm Configuration base Projection

Alarm Configuration SourceRelease se materializa como Projection base exacta.

PRE-SAVE validation y Materialization validation son capas distintas.

B.2 External Resolution vuelve a validar la revisión persistida contra dependencias actuales antes de producir artifacts operacionales.

## Tool Catalog

Command Center dispone de Tool Catalog V1 durable y de un read model para authoring.

Tool Catalog no forma parte del payload durable de Alarm Configuration y no reemplaza B.2.

B.2 requiere además current reconciliation qualification de las Tools referenciadas; el contrato exacto de ese input continúa OPEN.

## Alarm Engine CURRENT

Engine recibe configuración ejecutable materializada.

Runtime implementa:
- priority predominance por `priority_order`;
- Management suppression de lower-priority Rules independiente de `kind`;
- timer reappearance;
- Special Condition reappearance mediante `PlannedAlarm.reappearance_special_conditions`;
- level-trigger semantics para Special Condition;
- routing continuo durante Management suppression;
- durable Engine commits/WAL;
- materialized hot runtime snapshots por `priority_group`.

La Web no debe reimplementar estas reglas.

## Durable facts vs hot state

CURRENT Engine persistence mantiene dos superficies diferentes:

```text
Engine cycle
    |
    +--> durable commit facts / WAL
    |
    `--> GroupRuntimeSnapshot hot state
         runtime/state/groups/<priority_group>.json
```

`GroupRuntimeSnapshot` contiene estado necesario para continuidad/recovery, incluyendo occurrence, evaluation, management/deactivation effects, assignments y episode state.

No se congela `GroupRuntimeSnapshot` como contrato público de Delivery.

La futura frontera Engine → Delivery debe exponer estado operacional ya resuelto, sin obligar a Delivery a recalcular priority/lifecycle/management.

## Special Condition boundary

Alarm Configuration conserva:

```text
is_special_condition
reappearance.special_conditions
```

Engine consume:

```text
PlannedAlarm.reappearance_special_conditions
```

Engine no necesita `is_special_condition`.

B.2 valida/califica y materializa la referencia.

## Runtime Configuration vs deployed evaluator code

La configuración referencia lógica por key:

```text
family_key + evaluator_key
```

B.2 valida la existencia del evaluator, pero no serializa el callable.

Runtime une:

```text
RuntimeAlarmConfiguration
+ deployed AlarmEvaluatorRegistry
-> AlarmExecutionSession
```

`AlarmExecutionSession` no es el artifact persistido producido por B.2.

## Una resolución, dos artifacts

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
B.2 Resolution
    resolution_key
        |
        +--> Runtime Configuration Artifact
        `--> Delivery Configuration Artifact
```

Ambos artifacts comparten exactamente el mismo `resolution_key`.

La resolution es atómica:

```text
READY
-> ambos artifacts existen

BLOCKED
-> ninguno existe
```

No existe readiness operacional independiente por capability dentro de una misma resolución.

## READY vs EFFECTIVE

```text
B.2 READY
-> candidato coherente

Runtime Adoption succeeds
-> effective_resolution_key advances
```

Por tanto:

```text
READY != EFFECTIVE
```

Delivery sigue `effective_resolution_key` y nunca lidera Runtime.

## Visibility

Contrato authoring:

```text
VISIBLE
TRACE_ONLY
```

`TRACE_ONLY` significa ejecutar + trazar + no mostrar operacionalmente.

No mapear automáticamente a:

```text
PlannedAlarm.delivery_enabled=false
```

porque el flag CURRENT produce `SHADOW` y cambia priority/Management.

Esta reconciliación continúa OPEN para B.2/Delivery.

## Future Engine → Delivery operational boundary

PROJECT DIRECTION / NOT YET IMPLEMENTED:

Delivery no debe leer el WAL como API operacional ni recalcular prioridad desde Rules físicamente activas.

La frontera deseada es:

```text
Engine resolved current state
+ Delivery Configuration matching effective_resolution_key
        |
        v
Delivery
        |
        v
Alarm Live Projection
```

El schema concreto de `Engine resolved current state` todavía no está congelado.

El hot snapshot actual puede ser fuente interna para construir esa salida, pero no se declara equivalente al contrato de Delivery.

## Management input vs Management Projection

No confundir:

```text
Management input capture
-> entrada al Engine para evaluar acciones/decisiones
```

con:

```text
Management Projection
-> read-side histórico derivado de hechos durables ya resueltos por Engine
```

La autoridad sobre outcome `EFFECTIVE / ADDITIONAL / LATE` permanece en Engine.

## Management vs Live

Managed/deactivated no significa physical false.

Live Projection expresa estado operacional actual derivado.

Management Projection expresa acciones/decisiones históricas de gestión.

No mezclar ambas superficies.

## History / Analytics

History/Analytics consume hechos durables y no modifica Engine.

El boundary Engine → History/Analytics → Web permanece separado del foco B.2 actual.

No se implementa ni rediseña Analytics dentro de Configuration Materialization.

## Provenance

Runtime mantiene strings históricos:

```text
alarm_configuration_revision
tool_registry_revision
```

B.2 acordó identidad mínima:

```text
alarm_configuration_revision
confirmed_tool_catalog_revision
```

La implementación debe reconciliar el naming histórico mediante reemplazo limpio, sin adapters legacy permanentes.
