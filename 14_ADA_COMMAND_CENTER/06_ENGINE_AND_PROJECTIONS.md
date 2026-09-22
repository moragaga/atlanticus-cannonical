# ADA Command Center — Engine and Projections

Estado: **CURRENT / ENGINE RUNTIME CONTRACT STABILIZED / B.2 OPEN**

## Alarm Configuration base Projection

Alarm Configuration SourceRelease se materializa como Projection base exacta sin resolver dependencias externas.

External resolution pertenece a B.2.

## Tool Catalog

Command Center dispone de Tool Catalog V1 durable y de un read model para authoring.

Tool Catalog no forma parte del payload durable de Alarm Configuration y no reemplaza B.2.

## Alarm Engine CURRENT

Engine recibe configuración ejecutable materializada.

Después de los últimos incrementos, Runtime ya implementa:
- priority predominance por `priority_order`;
- Management suppression de lower-priority Rules independiente de `kind`;
- timer reappearance;
- Special Condition reappearance mediante `PlannedAlarm.reappearance_special_conditions`;
- level-trigger semantics para Special Condition;
- routing continuo durante Management suppression.

La Web no debe reimplementar estas reglas.

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

B.2 debe validar/calificar y materializar la referencia.

## Runtime vs Delivery

Runtime y Delivery deben derivar de la misma resolución/provenance.

Readiness puede diferir por capability.

Delivery no puede publicar referencias externas no resueltas.

Delivery no lidera la configuración EFFECTIVE; Runtime adoption determina EFFECTIVE.

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

Esta reconciliación pertenece a B.2/Delivery.

## Management vs Live

Managed/deactivated no significa physical false.

Live Projection expresa estado operacional actual derivado.

Management Projection expresa acciones/decisiones históricas de gestión.

No mezclar ambas superficies.

## History / Analytics

History/Analytics consume hechos durables y no modifica Engine.

El boundary Engine → History/Analytics → Web permanece separado del próximo foco B.2.

## Provenance

Runtime mantiene revision strings históricos:

```text
alarm_configuration_revision
tool_registry_revision
```

B.2 debe definir una resolución concreta y reconciliar provenance sin adapters legacy permanentes.
