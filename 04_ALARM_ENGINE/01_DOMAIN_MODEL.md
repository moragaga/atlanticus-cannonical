# Alarm Engine — Domain Model

Estado: **CURRENT — SHARED CONFIG CONTRACTS + OPERATIONAL CORE IMPLEMENTED**

## Ownership CURRENT

`ada-contracts-alarms==1.0.0` posee contratos compartidos de configuración/publicación, incluyendo `AlarmConfiguration`, `AlarmDefinition`, `AlarmConfigurationSnapshot` y schemas compartidos.

`ada-command-center-alarms-core==1.0.0` posee semántica operacional del Engine: evaluation, occurrence/episode, priority/lifecycle y state transitions.

No volver a concentrar ambos ownerships en Core.

## Conceptos

```text
Rule / AlarmDefinition
    configuración canónica publicada

PlannedAlarm
    forma resuelta para Runtime

Occurrence
    activación de una Rule

Episode
    lifecycle compartido dentro de priority_group

AlarmProjectionSnapshot
    read model current por Tool producido por Modeler
```

## Identidad

```text
AlarmIdentity(family_key, alarm_key)
```

- key estable;
- no usar `rule_key` separado;
- `rule_name` editable y único dentro de family;
- `display_name` requerido;
- `title` es metadata estática del artifact;
- `cause_template` sigue estático en el baseline actual.

La causa dinámica efectiva permanece OPEN.

## Configuración

- kind: `RISK | IMPACT`;
- criticality: `C1 | C2 | C3`;
- categorías: Ecology / Productivity / Safety / Costs;
- áreas: Mine / Plant, una o más;
- color semántico: RED / YELLOW;
- evaluator: `evaluator_key` + parámetros simples `str|float|bool`;
- prioridad: `priority_group + priority_order`.

## Visibilidad

```text
VISIBLE
TRACE_ONLY
```

TRACE_ONLY puede evaluarse y trazarse, pero no entra al snapshot visible del Modeler.

## Priority authority

Runtime es autoridad de prioridad.

Dispositions CURRENT:

```text
PREDOMINANT
DEACTIVATED
ECLIPSED
CASCADE_SUPPRESSED
```

Modeler no vuelve a ejecutar priority. Para el live baseline sólo proyecta `ACTIVE + PREDOMINANT` elegible para el destino.

`ranking` queda prohibido como concepto paralelo; la prioridad ordinal publicada es `priority_order`.

## Projection semantics

`operator_pool` es un read model de alarms elegibles/autoritativas para un Tool; no equivale a todas las alarmas `ACTIVE` del Runtime.

`operator_view` es la selección visible del Modeler sobre ese pool.

Estos son estados de proyección, no estados del Runtime.

## Reappearance / management

Los contratos históricos de reappearance/deactivation/management siguen perteneciendo a Runtime/lifecycle. El live baseline no implementa todavía las proyecciones `tracking_view` ni `inactive_reactivation_view`.

## Core boundaries

Core/shared domain no debe conocer:

```text
Cosmos transport
Dash/Flask
CSS/pixels
Web sessions
Tool Catalog discovery
container provisioning
```
