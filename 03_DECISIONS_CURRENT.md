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

## KPI Registry / Definition

CURRENT/FROZEN:

```text
KpiRegistry
KpiRegistryBinding
SourceKey('kpis')
ProjectionRecord[KpiRegistry]

KpiDefinition
KpiDefinitionConfiguration
KpiDefinitionCatalog
SourceKey('kpi-definitions')
ProjectionRecord[KpiDefinitionCatalog]
```

## REPROCESS_CURRENT

Implementado y CURRENT sólo en:

```text
KPI Runtime
KPI Historian
```

Delivery/Timeseries reprocess permanece:

```text
PROPOSED / DEFERRED / NOT AUTHORIZED
```

## Backend Registry consumption

Delivery y Timeseries leen directamente el KPI Registry durable desde Cosmos.

Cada proceso posee su reader y traducción interna. No existe librería compartida creada sólo
para deduplicar esta frontera.

## Collector — decisions CURRENT

```text
ADA-WEB-KPI-COLLECTOR-CAPABILITY
CLOSED / VERIFIED / CURRENT
```

Scheduling:

```text
Latest default       10 s
Timeseries default  120 s
Browser default      10 s
Latest priority      first when both due
```

Latest y Timeseries son superficies independientes. No se exige sincronización temporal exacta
ni atomicidad cross-document.

Compatibilidad server-side:

```text
(configuration_revision, tool_projection_revision)
```

Rules:

```text
Latest watermark regression    reject as STALE
Timeseries end regression       reject as STALE
wrong tool projection revision  INCOMPATIBLE
Timeseries vs current Latest     INCOMPATIBLE when compatibility differs
new incompatible Latest         drop cached Timeseries
missing document                retain last good state
invalid contract/source error   do not mutate state
```

## Component / Store ownership

CURRENT/FROZEN:

```text
ToolStructure.components
→ one logical ComponentStoreSnapshot per ToolComponent

Subcomponent
→ no own KPI Store

system destinations
→ no implicit Component Store
```

Cada browser store reúne Latest + Timeseries para ese Component.

## Browser merge

El browser nunca lee Cosmos. Consume snapshots del worker.

Para evitar regresión entre workers usa marcadores monotónicos independientes:

```text
Latest
revision + watermark_utc + configuration_revision

Timeseries
revision + end_utc + configuration_revision
```

Latest puede avanzar conservando Timeseries más nuevo ya presente en el browser cuando la
compatibilidad lo permite.

## Web Observability

CURRENT:

```text
Atlanticus Web owns WebObservability
→ registered in ServiceRegistry
→ modules consume via WEB_OBSERVABILITY_SERVICE_KEY
```

Collector policy:

```text
Delivery unavailable -> WARNING once per incident signature/source
Contract failure      -> ERROR once per incident signature/source
Other refresh failure -> ERROR once per incident signature/source
Runtime failure       -> CRITICAL
Recovery              -> clears source incident
```

No usar polling exitoso como telemetría periódica.

## Collector attachment

CURRENT:

```text
attach_ada_kpi_collector(WebApplicationDefinition, collector)
```

La función decora una definición existente; no convierte Generic Application en dependiente
obligatorio del collector.

Attachment duplicado se rechaza.

## Decisiones superseded por este cierre

```text
Collector exact intervals OPEN
SUPERSEDED

Collector read coherency OPEN
SUPERSEDED

Collector UI store wiring OPEN
SUPERSEDED

Collector capability PLANNED / NEXT
SUPERSEDED
```

Ahora son CURRENT los contratos implementados descritos arriba.

## Siguiente decisión operacional

No hay nueva arquitectura de Collector por decidir.

```text
ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION
PLANNED / NEXT
```

El siguiente incremento debe integrar las piezas existentes en la composición operacional real.
No inventar un segundo collector, adapter, schema, service o app para hacer el wiring.
