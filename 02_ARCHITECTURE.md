# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA.

## Configuration / Administration

Manager administra Source/Projection sólo para domains que realmente son Configuration Sources.

```text
Source      = Local | Blob | provider equivalente
Projection  = Local | Cosmos | provider equivalente
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

## KPI Registry CURRENT

La capability operacional de participación/delivery KPI es Registry, no el monolito histórico
`KPI Configuration`.

```text
scopes/ada/web/kpis/registry/
├── core
├── configuration
├── projection-local
└── projection-cosmos
```

Source:

```text
SourceKey('kpis')
```

Projection:

```text
ProjectionRecord[KpiRegistry]
```

Dependency exacta:

```text
Tool ProjectionTarget
        ↓
KPI Registry ProjectionTarget
```

## KPI Definition CURRENT

```text
scopes/ada/web/kpis/definition/
├── core
├── configuration
├── projection-local
└── projection-cosmos
```

Projection:

```text
ProjectionRecord[KpiDefinitionCatalog]
```

Dependency exacta:

```text
KPI Registry ProjectionTarget
        ↓
KPI Definition ProjectionTarget
```

## Backend KPI configuration boundary

Delivery y Timeseries consumen el KPI Registry durable CURRENT mediante readers propios de cada
proceso. No existen fallback legacy ni shared reader creado sólo por deduplicación.

## Backend recovery boundary

`REPROCESS_CURRENT` está implementado únicamente donde fue autorizado: KPI Runtime y Historian.

Nunca bypass:

```text
authority regression
lease
cancellation
fencing
write conflicts
```

## ADA KPI Collector CURRENT

Collector es una capability ADA Web separada:

```text
scopes/ada/web/kpis/collector
```

Flujo CURRENT:

```text
Latest Delivery Cosmos ─┐
                        ├─> AdaKpiCollector
Timeseries Delivery ────┘       │
                                ├─> immutable process snapshot
ToolStructure ------------------┤
Tool projection revision -------┘
                                ↓
                     one logical store per Component
                                ↓
                         browser cache reads
```

Fronteras:

```text
Cosmos
→ only reader/collector side

Browser
→ cache only
→ never Cosmos

Generic Application
→ remains usable without collector

Subcomponent
→ never gets its own KPI store
```

Compatibilidad:

```text
configuration_revision + tool_projection_revision
```

No existe atomicidad requerida entre Latest y Timeseries. Latest puede avanzar primero mientras
Timeseries compatible anterior permanece. Un cambio incompatible de Latest invalida Timeseries.

## Collector Web composition

Atlanticus Web crea `WebObservability` y la registra en su `ServiceRegistry` mediante:

```text
WEB_OBSERVABILITY_SERVICE_KEY
```

Los módulos pueden declararla en `requires_services` sin crear globals.

Collector se adjunta a una definición Web ya resuelta:

```text
WebApplicationDefinition
    ↓
attach_ada_kpi_collector
    ↓
WebApplicationDefinition + collector module + wrapped layout
```

No introducir dependencia inversa desde Generic Application hacia Collector.

## Lifecycle

```text
one cache/poller per worker PID
latest interval default     10 s
timeseries interval default 120 s
browser interval default    10 s
```

Public infrastructure paths no arrancan el poller:

```text
/health/
/assets/
/.auth/
```

## Siguiente frontera arquitectónica

La capability está cerrada. Lo siguiente es wiring, no diseño nuevo:

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

Resolver en la composición operacional existente:

```text
published/current ToolConfiguration
ToolStructure
Tool projection revision
Cosmos connection/client
AdaKpiCollector
attach_ada_kpi_collector
```

No crear un nuevo servicio remoto, schema intermedio, store por Subcomponent ni aplicación
paralela salvo evidencia explícita posterior.

## Reglas congeladas

```text
LEGACY                      REMOVE
ADAPTERS / SHIMS / ALIASES FORBIDDEN
DOUBLE CONTRACT             FORBIDDEN
OLD SCHEMA READERS          FORBIDDEN IN CURRENT RUNTIME
revision -> ProjectionTarget reconstruction REMOVE
expected_source_revision    REMOVE
```
