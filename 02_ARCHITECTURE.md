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

Ownership:

```text
core
    KpiRegistry
    KpiRegistryBinding

configuration
    Source lifecycle
    Projection builder/serializer
    Web editor

projection-local
    durable local ProjectionStore[KpiRegistry]

projection-cosmos
    Cosmos ProjectionStore[KpiRegistry]
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

Cosmos:

```text
logical_id
ada.kpis.registry.projection

default physical name
ada-kpi-registry-projection

partition
/partition_key

TTL
None
```

No existe contrato CURRENT:

```text
ada.web.kpis.configuration
KpiConfiguration
KpiConfigurationBinding
```

## KPI Definition CURRENT

```text
scopes/ada/web/kpis/definition/
├── core
├── configuration
├── projection-local
└── projection-cosmos
```

Ownership:

```text
core
    KpiDefinition
    KpiDefinitionConfiguration
    KpiDefinitionCatalog

configuration
    Source lifecycle
    Projection builder/serializer
    Web editor

projection-local
    durable local ProjectionStore[KpiDefinitionCatalog]

projection-cosmos
    Cosmos ProjectionStore[KpiDefinitionCatalog]
```

Source:

```text
SourceKey('kpi-definitions')
resource = kpis/definition.json.gz
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

Cosmos:

```text
logical_id
ada.kpis.definition.projection

default physical name
ada-kpi-definition-projection

partition
/partition_key

TTL
None
```

## Backend KPI configuration boundary

Delivery y Timeseries deben consumir el KPI Registry durable CURRENT.

El documento backend histórico:

```text
ada_kpi_configuration_projection
```

no es el contrato objetivo.

No crear:

```text
legacy reader
dual schema reader
shim
alias
secondary projection document
```

El cambio de consumer debe hacerse en la frontera del proceso existente.

## Backend recovery boundary

`REPROCESS_CURRENT` no es debug.

Para jobs autorizados significa únicamente:

```text
current checkpoint
→ volver a materializar usando authority upstream current
```

Nunca bypass:

```text
authority regression
lease
cancellation
fencing
write conflicts
```

Autorizado/PLANNED:

```text
KPI Runtime
Historian
```

No autorizado en esta secuencia:

```text
Latest Delivery reprocess
Timeseries Delivery reprocess
```

## ADA Generic Collector

Collector no se define por coincidencia terminológica con Producer.

Su cierre queda bloqueado hasta calificar la cadena backend KPI.

Después debe mapearse:

```text
Component
→ collector contract
→ existing physical data capability
```

y sólo introducir una responsabilidad nueva si el gap es real.

## Reglas congeladas

```text
LEGACY                      REMOVE
ADAPTERS / SHIMS / ALIASES FORBIDDEN
DOUBLE CONTRACT             FORBIDDEN
OLD SCHEMA READERS          FORBIDDEN IN CURRENT RUNTIME
revision -> ProjectionTarget reconstruction REMOVE
expected_source_revision    REMOVE
```
