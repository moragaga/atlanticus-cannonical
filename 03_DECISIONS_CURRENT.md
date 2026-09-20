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

Default:

```text
false
```

Nunca bypass:

```text
authority ordering
watermark regression checks
lease
cancellation
fencing
write conflict detection
```

Delivery/Timeseries reprocess permanece:

```text
PROPOSED / DEFERRED / NOT AUTHORIZED
```

## Backend Registry consumption

Delivery y Timeseries leen directamente el KPI Registry durable desde Cosmos.

Cada proceso posee su reader y traducción interna. No existe librería compartida creada sólo para deduplicar esta frontera.

```text
shared Registry reader implementation
SUPERSEDED

independent process-owned readers
CURRENT
```

## Cosmos configuration

CURRENT:

```text
connection credentials + database name
→ external configuration / ENV

container identity + topology + document contract
→ internal process contract
```

Cada proceso:

```text
consumed Registry container
→ validate/read only
→ never provision

owned output container
→ ensure once at startup
→ never ensure per iteration
```

La database permanece infraestructura externa; estos procesos no asumen ownership de crearla.

## Collector

El gate KPI backend ya está cerrado.

```text
ADA-GENERIC-COLLECTOR-CLOSURE
PLANNED / NEXT
```

Decisión vigente:

```text
Latest y Timeseries deben tener intervalos de lectura distintos.
Latest es prioritario.
```

OPEN, no decidido todavía:

```text
intervalos numéricos
mecanismo de sincronización entre ambos reads
forma exacta de actualización de stores UI
shape exacta de composición Collector
```

El contrato Tool CURRENT debe ser la base para resolver component/destination mapping; no crear un contrato paralelo.
