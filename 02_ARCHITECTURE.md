# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA.

## Configuration / Administration

Manager administra Source/Projection sólo para dominios que realmente son Configuration Sources.

```text
Source      = Local | Blob
Projection  = Local | Cosmos
```

Los providers son ejes independientes.

```text
local + local
blob  + cosmos
blob  + local
local + cosmos
```

Projection representa un release exacto:

```text
ProjectionTarget
= SourceKey
+ SourceReleaseRef
+ dependencies
```

No reconstruir `ProjectionTarget` desde revision textual.

No mantener contratos paralelos para transición.

## Namespace de persistencia ADA

CURRENT:

```text
AdaStorageNamespace
├── application_namespace
└── tool_namespace
```

Separar siempre:

```text
connection
physical container
application namespace
tool namespace
SourceKey
```

Ejemplo lógico:

```text
conciencia_situacional/
├── users/
└── operaciones_integradas/
    ├── sources/
    └── projections/
```

`users` pertenece al nivel global de aplicación.

Los domains de Tool pertenecen a `<application>/<tool>`.

`SourceStore` agrega internamente `sources/<SourceKey>`.

## Tool Configuration / Projection

Tool Configuration es ADA-specific.

CURRENT:

```text
ToolSourceService
ToolProjectionBuilder
ProjectionRecord[ToolConfiguration]
LocalToolProjectionStore
CosmosToolProjectionStore
```

El namespace lógico de deployment no modifica `SourceKey`.

Cosmos Tool Projection usa:

```text
partition_key = <application>/<tool>
```

Local Tool Projection usa:

```text
<base>/<application>/<tool>/projections
```

## Tool persistence composition

Capability CURRENT:

```text
scopes/ada/web/tools/persistence
```

Composición:

```text
ToolPersistenceSettings
        ↓
compose_tool_persistence
        ↓
ToolPersistenceComposition
├── SourceStore
├── ProjectionStore[ToolConfiguration]
└── SourceProjectionService[ToolConfiguration]
```

Construir esta composición no debe abrir ni consultar servicios externos.

Operaciones separadas:

```text
resolve_active_tool_projection()
→ runtime read from durable Projection

project_current_tool_source()
→ Source current -> exact Projection
```

Estados:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

## Application availability boundary

Invariante congelada:

```text
APPLICATION EXISTENCE
!= CONFIGURATION EXISTENCE
!= INFRASTRUCTURE AVAILABILITY
!= DATA AVAILABILITY
```

Ausencia de Source/Projection/KPI data es estado funcional válido.

Falla de conectividad de una dependencia debe quedar confinada a esa capability.

Errores/contratos inválidos deben permanecer diagnosticables; resiliencia no significa
ocultarlos.

La aplicación real todavía no consume esta composición; ese wiring es el siguiente incremento.

## KPI Registry / Definition

Permanecen CURRENT sus contratos durables y exact dependencies.

No reabrirlos en el bootstrap ADA Generic.

## ADA KPI Collector CURRENT

Collector permanece una capability ADA Web separada.

```text
Latest Delivery Cosmos ─┐
                        ├─> AdaKpiCollector
Timeseries Delivery ────┘
ToolStructure ------------------┐
Tool projection revision -------┘
```

Fronteras congeladas:

```text
1 ToolComponent = 1 logical KPI Store
Subcomponent != Store
browser = cache only
Generic Application usable without Collector
```

## Próxima frontera arquitectónica

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

Debe consumir los contratos CURRENT; no crear nuevos Source/Projection providers ni otra
aplicación paralela.

Orden:

```text
Web settings/environment
→ AdaStorageNamespace
→ Source/Projection provider clients
→ ToolPersistenceComposition
→ resolve_active_tool_projection
→ ADA Generic composition/runtime
```

Después, cuando exista Tool Projection READY:

```text
Tool Structure
→ Collector
→ Latest / Timeseries states
```

La ausencia de Tool/KPI data o la indisponibilidad de provider no debe redefinir la existencia
de la Web.
