# ADA Command Center — Engine and Projections

Estado: **CURRENT / ALARM CONFIGURATION BASE PROJECTION + TOOL CATALOG PRODUCER IMPLEMENTED / B.2 OPEN**

## Alarm Configuration Projection CURRENT

Existe una Projection base bajo:

```text
scopes/ada-command-center/web/alarms/configuration
```

Materializa una `SourceRelease` exacta como `AlarmConfiguration`:

```text
Alarm Configuration SourceRelease
        ↓
AlarmConfigurationProjectionBuilder
        ↓
ProjectionStore[AlarmConfiguration]
```

Su `ProjectionTarget` no declara dependencias externas.

```text
ProjectionTarget.dependencies == ()
```

Esto es intencional: external resolution pertenece a B.2, no a la base projection.

## Tool Catalog producer CURRENT

Command Center ya dispone de una revisión consolidada de topología Tool:

```text
Tool Projection inputs
→ ToolCatalogConsolidator
→ ToolCatalogSnapshot
→ Blob CURRENT
```

El catálogo tiene lifecycle/revisión independiente de Alarm Configuration.

Existe además un read model backend-only para authoring. Ninguno de los dos reemplaza B.2.

## Alarm Engine

Alarm Engine recibe configuración ejecutable materializada.

La persistencia de Alarm Configuration puede preceder a la resolución completa de dependencias
externas. B.2 debe separar readiness por capability.

Una referencia Tool no resuelta no bloquea necesariamente Runtime si el evaluator y los datos
requeridos son ejecutables sin esa referencia.

La Web no resuelve:

- evaluator execution;
- priority;
- lifecycle;
- Special Cascade;
- reappearance;
- Message catalogs;
- deactivation authorization;
- routing execution.

## Runtime vs Delivery readiness

Runtime y Delivery deben derivar de la misma resolución/provenance, pero no toda dependencia afecta
a ambos de la misma forma.

Ejemplo:

```text
Alarm Configuration valid
Evaluator available
Tool visual target unresolved

→ Runtime may be READY
→ Delivery target is NOT READY
```

Delivery no debe despachar hacia una referencia externa no resuelta.

Delivery no puede liderar la configuración EFFECTIVE de Runtime.

`ResolvedAlarmConfiguration`, Runtime materialization y Delivery materialization permanecen
PLANNED; no existen en `main` al checkpoint de este cierre.

## Runtime CURRENT a reconciliar posteriormente

El runtime existente todavía usa contratos históricos con:

```text
alarm_configuration_revision: str
tool_registry_revision: str
```

en `PlannedAlarm`, `AlarmExecutionSession` y adoption.

B.2 deberá reconciliar estos revision strings con provenance CURRENT, sin introducir adapters legacy
ni doble contrato.

`AlarmEvaluatorRegistry` CURRENT resuelve por `(family_key, evaluator_key)` y no expone actualmente
una identidad/revisión de registry. La necesidad exacta de provenance para evaluator resolution
permanece OPEN para B.2.

## Live Projection

Pregunta:

> ¿Qué ocurre operacionalmente ahora?

Se materializa después de evaluation/lifecycle/priority/management/deactivation.

Managed o deactivated no significa que la condición física haya terminado.

## Management Projection

Pregunta:

> ¿Qué acciones de gestión realizaron los usuarios y cómo terminaron?

Es histórica y está orientada a management/deactivation.

No reemplaza Live.

## History / Analytics

Command Center necesita una frontera derivada para explicar la historia operacional completa.

Debe poder conservar provenance de:

- Alarm Source revision;
- resolution identity futura;
- Tool Catalog revision usada por esa resolución.

Nombre/API/storage todavía no congelados.

Este hito no modifica el Analytics Boundary existente.
