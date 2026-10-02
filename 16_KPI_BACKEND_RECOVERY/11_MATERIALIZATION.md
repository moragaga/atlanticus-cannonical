# KPI Backend Recovery — KPI Materialization

Estado: **CLOSED / VERIFIED / CURRENT**

## Purpose

KPI Materialization localiza por Tool el KPI Registry durable para que los consumidores backend no necesiten leer configuración directamente desde Cosmos durante su ejecución.

No consolida Registries globalmente.
No produce una configuración resuelta común.

## Connections

Input:

```text
config/connections.json
```

Cada key es un `tool_key` real y resuelve una named Cosmos connection.

Materialization lee connections una vez al iniciar el proceso.

## Source Registry

Por Tool:

```text
container       = ada-kpi-registry-projection
partition value = kpis
document_type   = ada_kpi_registry_projection_record
schema_version  = 1
source_key      = kpis
```

Registry payload binding:

```text
kpi_key
destination_keys
latest_enabled
series_enabled
series_hours
```

`series_hours`:

```text
1..24 iff series_enabled=true
None iff series_enabled=false
```

## Local authority

Path:

```text
<VOLUMEN_PATH>/ada-kpi-engine/materialization/registries/<tool_key>.json
```

El documento local preserva el Registry completo e inyecta únicamente:

```text
tool_key
```

en la raíz.

No persistir secretos ni CosmosSettings resueltos.

## Update semantics

Materialization procesa Tools secuencialmente.

Por Tool:

```text
read durable Registry
validate
materialize full document
compare with local
atomic replace only when changed
```

Failure de una Tool:

```text
preserve its last-known-good
continue remaining Tools
raise iteration error after processing all real failures
```

Configured Tool removida:

```text
remove corresponding local JSON
```

## Readiness

Missing Registry document no es configuración inválida.

CURRENT:

```text
KpiMaterializationRegistryPending
→ no process failure
→ set_next_iteration_delay(30)
```

Real Cosmos/acquisition/contract/store errors continúan siendo failures.

## Polling

```text
POLL_INTERVAL_SECONDS=30
```

## Consumers

CURRENT:

```text
Latest Delivery
```

PLANNED:

```text
Timeseries Delivery
```

Cada consumidor:

```text
reads materialized Registries only while becoming ready
freezes them for process lifetime
does not hot-reload
```

## Qualification

```text
kpi-materialization-runtime    14 passed
Ruff / format                  PASS
mirrors                        PASS
public import                  PASS
wheel build                    PASS
```
