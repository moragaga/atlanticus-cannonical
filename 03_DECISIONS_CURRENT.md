# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

```text
Python 3.14.7
uv, no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
no double contract
one focus per increment
```

## Storage namespace

CURRENT / FROZEN:

```text
physical container != application namespace != tool namespace != SourceKey
```

`SourceStore` es dueño del segmento `sources/`.

No reconstruir rutas globales mediante `../`.

## Tool Projection persistence

CURRENT / FROZEN:

```text
ProjectionRecord[ToolConfiguration]
LocalToolProjectionStore
CosmosToolProjectionStore
```

Runtime activo:

```text
resolve_active_tool_projection()
→ durable Tool Projection
```

Source participa sólo cuando un workflow necesita seleccionar/proyectar una Source release.

## Provider composition

CURRENT / FROZEN:

```text
Source provider      local | blob
Projection provider  local | cosmos
```

Combinaciones independientes soportadas:

```text
local + local
blob  + cosmos
blob  + local
local + cosmos
```

## Availability rule

CURRENT / FROZEN:

```text
APPLICATION EXISTENCE
!= TOOL CONFIGURATION EXISTENCE
!= EXTERNAL INFRASTRUCTURE AVAILABILITY
!= BUSINESS DATA AVAILABILITY
```

Tool resolution:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

Ausencia o indisponibilidad de una capability no equivale automáticamente a caída global de Web.

## ADA Generic bootstrap

CURRENT / FROZEN:

```text
environment / .env
→ provider/client settings
→ AdaStorageNamespace
→ ToolPersistenceComposition
→ resolve_active_tool_projection()
→ ADA Generic composition
```

La ruta startup in-process basada en Source fue eliminada.

No reintroducir:

```text
_StartupToolProjectionStore
resolve_current_tool_projection
startup Source -> Projection reconstruction
```

## KPI Collector decisions

CURRENT / FROZEN:

```text
Latest polling      10 s default
Timeseries polling 120 s default
Browser refresh     10 s default
Latest priority
1 ToolComponent = 1 logical/browser store
Subcomponent != Store
browser cache only
```

Collector runtime wiring usa una conexión de consumo KPI separada de Tool Projection.

No asumir que ambas persistencias comparten Cosmos connection.

## Operational render boundary

CURRENT / FROZEN:

```text
CONFIGURATION DETERMINES STRUCTURE
DATA DETERMINES RUNTIME STATE
```

`OperationalRenderBinding` representa únicamente estructura:

```text
ToolStructure
→ OperationalComponentBinding
→ ToolComponent
```

No contiene:

```text
ComponentStoreSnapshot
KPI state
Collector state
browser state
```

`AdaKpiCollector` no expone `operational_render_binding` y no depende del paquete
`ada-web-operational-render-binding`.

## Data delivery boundary

CURRENT / FROZEN:

```text
ADA Generic
→ Tool resolution
→ ToolStructure
→ Collector
→ process cache
→ dcc.Store por ToolComponent
→ END GENERIC DATA DELIVERY
```

Desde esa frontera:

```text
developer / concrete Tool application
→ conecta data operacional
→ decide layout/render visual concreto
```

ADA Generic no posee un body universal de Tool.

No crear:

```text
generic mandatory body renderer
KPI -> render adapter
duplicate UI store
tool-specific layout inside generic core
```

La misma regla orientará Alarm: core entrega contrato/estado; el consumidor decide representación.

## Decisiones anteriores reemplazadas o refinadas

```text
"Collector debe conectarse directamente desde Tool Source"
SUPERSEDED
```

La ruta CURRENT consume Tool Projection durable.

```text
"in-process startup Tool Projection es la ruta operacional"
SUPERSEDED / REMOVED
```

```text
"Source debe estar disponible para resolver Tool runtime"
SUPERSEDED
```

```text
"OperationalRenderBinding empareja ToolComponent + ComponentStoreSnapshot"
SUPERSEDED / REMOVED
```

```text
"Collector expone operational_render_binding sobre sus stores"
SUPERSEDED / REMOVED
```

```text
"ADA Generic debe conectar automáticamente los KPI stores a un body genérico"
SUPERSEDED / NOT REQUIRED
```

La frontera final es entrega de datos al desarrollador, no ownership de visualización.

## Stage closure

```text
ADA-GENERIC-STAGE-1
CLOSED / VERIFIED / CURRENT
```

No existe una etapa adicional conocida de ADA Generic antes de la entrega de datos.

Nuevos increments sólo deben abrirse si una Tool concreta demuestra un finding real.

## Next decision boundary

```text
ADA-COMMAND-CENTER-ALARM-CONFIGURATION
PLANNED / NEXT
```

Primero auditar implementación y contratos actuales.

No rediseñar Alarm Engine ni inventar storage/schema sin finding.
