# ADA Command Center — Engine and Projections

Estado: **CURRENT / REFINED + ANALYTICS BOUNDARY CANDIDATE**

## Alarm Engine

Alarm Engine recibe configuración ejecutable materializada.

La persistencia de Alarm Configuration puede preceder a la resolución completa de dependencias externas. B.2 debe separar readiness por capability.

Una referencia Tool no resuelta no bloquea necesariamente Runtime si el evaluator y los datos requeridos son ejecutables sin esa referencia.

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

Runtime y Delivery derivan de la misma resolución/provenance, pero no toda dependencia afecta a ambos de la misma forma.

Ejemplo:

```text
Alarm Configuration valid
Evaluator available
Tool visual target unresolved

→ Runtime may be READY
→ Delivery target is NOT READY
```

Una Rule que puede evaluarse puede generar Occurrence/Journey/Evidence durante marcha blanca aunque un destino visual/routing externo todavía no esté disponible.

Delivery no debe despachar hacia una referencia externa no resuelta.

Delivery no puede liderar la configuración EFFECTIVE de Runtime.

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

Command Center necesita una tercera frontera conceptual para explicar la historia operacional completa.

Debe combinar:

- Occurrence/Episode;
- Journey;
- Evidence;
- priority transitions;
- management;
- deactivation;
- routing/assignment;
- Alarm Source revision;
- resolution identity/provenance;
- Tool Catalog revision/topology provenance.

Nombre/API/storage todavía no congelados.

No asumir aún si será:

- una History Projection;
- una Analytics Projection;
- ambas.

## Regla

History/Analytics es read model derivado.

No modifica Engine state y no reemplaza:

- Durable Engine;
- Live Projection;
- Management Projection.
