# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / LOCALLY USED / METADATA NOT YET GLOBALLY ALIGNED |
| `python:3.14.7-slim-trixie` | DECIDED / NOT YET QUALIFIED GLOBALLY |
| `uv`, no pip normal | CURRENT |
| Definir contratos antes que consumidores | CURRENT |
| Backend antes que frontend | CURRENT, salvo precondición durable ya cerrada para KPI |
| Cutover raíz limpio | CURRENT |
| No shims/adapters/aliases legacy | FROZEN |
| No doble contrato | FROZEN |
| Tests no son autoridad sobre contracts SUPERSEDED | FROZEN |
| Un foco por incremento | FROZEN |

## KPI Registry

CURRENT/FROZEN:

```text
KpiRegistry
KpiRegistryBinding
SourceKey('kpis')
ProjectionRecord[KpiRegistry]
```

Estructura:

```text
registry/core
registry/configuration
registry/projection-local
registry/projection-cosmos
```

SUPERSEDED:

```text
ada.web.kpis.configuration
KpiConfiguration*
```

## KPI Definition

CURRENT/FROZEN:

```text
KpiDefinition
KpiDefinitionConfiguration
KpiDefinitionCatalog
SourceKey('kpi-definitions')
ProjectionRecord[KpiDefinitionCatalog]
```

Estructura:

```text
definition/core
definition/configuration
definition/projection-local
definition/projection-cosmos
```

Dependency:

```text
Registry ProjectionTarget
→ Definition ProjectionTarget
```

## KPI backend configuration consumption

DECIDED:

```text
kpi-delivery
kpi-timeseries-delivery
```

deben dejar de consumir el projection document legacy y leer el Registry durable publicado en
Cosmos.

No compatibility reader.

No secondary schema.

No duplicar Registry en otro documento backend.

## REPROCESS_CURRENT

Nombre contractual:

```text
REPROCESS_CURRENT
```

Default:

```text
false
```

DECIDED / PLANNED:

```text
KPI Runtime
Historian
```

Semántica:

```text
false
→ comportamiento normal

true
→ bypass exclusivamente del shortcut "already current"
```

Se preservan:

```text
authority
watermark ordering
lease
cancellation
fencing
conflict detection
```

Para Historian, forced-current debe releer desde inicio hasta KPI committed current.

## Delivery/Timeseries reprocess

La propuesta histórica de dar `REPROCESS_CURRENT` también a Delivery y Timeseries queda:

```text
PROPOSED / DEFERRED / NOT AUTHORIZED
```

No mezclar con el cutover de consumo Registry.

## Collector

```text
ADA-GENERIC-COLLECTOR-CLOSURE
BLOCKED
```

Se abre sólo después de cerrar la cadena KPI backend.

## Testing

Automatizar:

```text
behavior
contracts
invariants
regressions
critical flows
```

No crear tests cuyo único objetivo sea:

```text
CSS visual
existencia/no existencia de functions/classes
estructura interna
source token presence
```

## Siguiente foco

```text
KPI-RUNTIME-REPROCESS-CURRENT
PLANNED / NEXT
```
