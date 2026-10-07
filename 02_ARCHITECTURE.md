# Atlanticus — Architecture

Estado: **CURRENT — MODULAR RUNTIME + CONFIGURATION-OWNED OPERATIONAL PRESENTATION**

## Regla principal

Atlanticus es modular y reusable. ADA y Command Center son consumidores.

## Clean cutover rule

```text
contracts before consumers
clean replacement
no legacy aliases
no dual source of truth
no compatibility path without explicit decision
```

## Operational Data

El contrato CURRENT previo permanece:

```text
DataInputSpec
    ↓
DataInputPlanner
    ↓
DataInputLoadPlan
    ↓
DataInputLoader
    ↓
LoadedDataInputs
    ↓
DataInputContext
```

## Web operational presentation — FROZEN

Para superficies operacionales no-alarmas:

```text
Tool/configuration/bindings
    determine structure and existence

runtime data
    determines state and content
```

Principio:

```text
configured UI does not disappear because Delivery is absent
```

La estructura de presentación no debe quedar accidentalmente bajo ownership de un proveedor de datos.

## KPI presentation store boundary — FROZEN

Browser store materialization y Delivery polling son responsabilidades separadas.

```text
ToolProjection READY
    ↓
ToolStructure
    ↓
presentation stores
```

independientemente de:

```text
KPI Delivery Cosmos configured?
```

Cuando Delivery existe:

```text
Collector polling
    ↓
updates the already-materialized stores
```

Cuando Delivery no existe:

```text
stores remain with an empty canonical presentation snapshot
```

Cuando Tool está UNCONFIGURED:

```text
no operational structure
→ no operational presentation stores
```

## Authoring / Normal — FROZEN

`AUTHORING` y `NORMAL` no construyen árboles de UI distintos.

```text
same configuration
same component tree
same stores
same runtime callbacks
```

`ContentStatePresentationMode` sólo modifica cómo se presenta una degradación.

Para wrappers operacionales:

```text
NORMAL
→ overlay visible when state requires it

AUTHORING
→ overlay hidden
→ real underlying component remains available for design
```

## Global Indicator collection boundary — FROZEN

La unidad de estado operacional es la colección completa montada por el runtime, no cada celda.

```text
DashboardGlobalIndicatorsRuntimeBinding
    tool_key
    indicators[]
    content_state
```

Cada binding individual conserva:

```text
definition
presentation scopes
```

No añadir `ContentState` por indicador para modelar una colección que se inyecta como una unidad.

No usar estados de DisplayValue como sustituto del estado de la colección.

## Global Indicator generic ownership — FROZEN

`ada-web-ui-global-indicator` posee comportamiento visual reusable:

```text
component geometry
actual / plan compact alignment
responsive sizing
generic placement
mobile two-column layout
mobile vertical growth
desktop full-height layout
generic dividers
```

Un consumidor puede envolver un GI con `.ada-global-indicator-placement` para metadata sin reimplementar sizing.

## Integrated Operations ownership — FROZEN

IO no redefine el responsive genérico.

IO puede poseer:

```text
product catalog
MINE / PLANT scopes
scope metadata
scope filtering
header-host adaptation
product-specific overrides justified by IO
```

IO no puede mover su semántica de scopes al componente genérico.

## Alarm exception — FROZEN

La regla configuration-owned presentation no se interpreta como:

```text
configured AlarmDefinition
→ permanent visible alarm card
```

Alarmas tienen semántica de dominio propia:

```text
AlarmDefinition
    ↓ evaluation/lifecycle
Occurrence / Episode
    ↓ projections
Web alarm visibility
```

La configuración define reglas y targets; lifecycle/projections determinan la presencia operacional visible.

## Operational header boundary

El header puede componer:

```text
branding
global indicators
alarm-management
alarm-status
```

Sizing final entre las cuatro superficies debe evaluarse cuando todas estén montadas.

No fijar proporciones definitivas usando sólo un subconjunto de slots.

## Distributed process resources

Las decisiones CURRENT previas de `deployment.resources.json` permanecen sin cambio por este hito.

## Python baseline

Project target:

```text
Python 3.14.7
python:3.14.7-slim-trixie
```

Web packages CURRENT observados:

```text
requires-python == 3.14.2
```

La migración permanece separada.
