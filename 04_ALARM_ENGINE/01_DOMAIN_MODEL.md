# Alarm Engine — Domain Model

Estado: **CURRENT — SHARED CONFIG CONTRACTS + OPERATIONAL DOMAIN PRESERVED / ENGINE MIGRATION PLANNED**

## Ownership CURRENT

`ada-contracts-alarms==1.0.0` posee contratos compartidos de configuración/publicación, incluyendo `AlarmConfiguration`, `AlarmDefinition`, `AlarmConfigurationSnapshot` y schemas compartidos.

`ada-command-center-alarms-core==1.0.0` continúa siendo el owner operacional documentado del baseline actual: evaluation, occurrence/episode, priority/lifecycle y state transitions.

Dirección PLANNED posterior:

```text
operational engine target
→ ada-alarm-engine
```

La migración no debe volver a concentrar contratos compartidos dentro del engine.

## Conceptos congelados

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

## Configuración CURRENT

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

Modeler no vuelve a ejecutar priority.

Para el live baseline sólo proyecta `ACTIVE + PREDOMINANT` elegible para el destino.

`ranking` queda prohibido como concepto paralelo; la prioridad ordinal publicada es `priority_order`.

## Projection semantics

`operator_pool` es un read model de alarms elegibles/autoritativas para un Tool; no equivale a todas las alarmas `ACTIVE` del Runtime.

`operator_view` es la selección visible del Modeler sobre ese pool.

Estos son estados de proyección, no estados del Runtime.

## Reappearance / management

Los contratos históricos de reappearance/deactivation/management siguen perteneciendo a Runtime/lifecycle.

El live baseline no implementa todavía las proyecciones `tracking_view` ni `inactive_reactivation_view`.

## Core / Engine boundaries

Shared contracts y engine no deben conocer:

```text
Dash/Flask
CSS/pixels
Web sessions
header geometry
KPI presentation layout
Tool Catalog discovery by UI
container provisioning
```

El engine puede depender de contratos técnicos necesarios para ejecutar, pero no de decisiones de composición Web.

## Migration constraints — FROZEN FOR NEXT ENGINE DESIGN

El próximo rediseño/migración del motor puede cuestionar campos existentes, pero no puede borrarlos silenciosamente.

Proceso obligatorio:

```text
1. inventory current implementation
2. inventory current shared contracts
3. compare with frozen alarm decisions
4. identify concrete consumers
5. classify each candidate field:
       keep
       move ownership
       supersede
       remove
6. freeze resulting contract
7. migrate cleanly
```

Target package:

```text
ada-alarm-engine
```

No asumir que el rename resuelve ownership.

No mover de vuelta `AlarmDefinition` al engine.

No introducir campos de UI para facilitar una card.

## Web visibility invariant

La existencia de `AlarmDefinition` no obliga a una card visible.

```text
configuration
    ↓
evaluation
    ↓
Occurrence / Episode
    ↓
priority / lifecycle
    ↓
live projection
    ↓
Web
```

Esta frontera debe preservarse al crear la primera card de alarmas.
