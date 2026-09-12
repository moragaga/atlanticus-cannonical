# ADA Command Center — Engine and Projections

Estado: **CURRENT/FROZEN + ANALYTICS BOUNDARY CANDIDATE**

## Alarm Engine

Recibe configuración ya resuelta.

La Web no resuelve:

- evaluators;
- priority;
- lifecycle;
- Special Cascade;
- reappearance;
- Message catalogs;
- deactivation authorization;
- routing.

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
- configuration revision;
- Tool topology revision.

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
