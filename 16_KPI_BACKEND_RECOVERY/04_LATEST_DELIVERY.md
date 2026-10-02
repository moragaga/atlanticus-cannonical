# KPI Backend Recovery — Latest Delivery

Estado: **CLOSED / VERIFIED / CURRENT**

## Reemplazo de diseño

SUPERSEDED:

```text
Delivery
→ una conexión Cosmos global
→ leer KPI Registry directamente desde Cosmos
→ un checkpoint global
→ publicación secuencial
```

CURRENT:

```text
Delivery START
→ leer config/connections.json una vez
→ resolver named Cosmos connections una vez
→ esperar Registries materializados si aún no están listos
→ congelar configuración por Tool para la vida del proceso

RUNNING
→ observar KPI committed watermark local
→ leer un único committed evaluation batch cuando hay cambio
→ construir snapshot por Tool
→ publicar Tools en paralelo
→ checkpoint independiente por Tool exitoso
```

## Materialized Registry input

Latest Delivery no lee el Registry directamente desde Cosmos.

Consume:

```text
<VOLUMEN_PATH>/ada-kpi-engine/materialization/registries/<tool_key>.json
```

Cada documento conserva el Registry completo e incorpora `tool_key` en la raíz.

El set materializado debe converger exactamente con las Tools configuradas.

Si todavía no converge:

```text
status = materialization_pending
next iteration delay = 30 s
```

Una vez que converge, la configuración queda frozen durante la ejecución.
No hay hot reload.

Un Registry inválido es error de configuración, no readiness.

## Polling normal

```text
POLL_INTERVAL_SECONDS=1
```

El chequeo normal usa estado local y no debe consultar Cosmos cuando no existe trabajo pendiente.

## Global KPI watermark

KPI Runtime produce un único batch global por watermark.

Latest Delivery:

```text
new committed watermark
→ read evaluation batch ONCE
→ common published_at_utc
→ project all pending Tool snapshots
```

No existe replay de snapshots intermedios.

Si Runtime avanzó de `10:25` a `10:35` mientras Delivery estaba detenido, Delivery publica directamente el snapshot correspondiente a `10:35`.

## Paralelismo

Publication es paralela por Tool mediante un pool acotado.

```text
KPI_DELIVERY_MAX_WORKERS=2
```

default actual.

Los workers realizan publicación Cosmos.

El hilo principal conserva:

```text
lease checks
checkpoint commit
runtime context mutation
failure aggregation
```

No usar `dispatch` como concepto arquitectónico de este proceso.
Operational Data Dispatch es otra capability independiente.

## Checkpoint por Tool

State key:

```text
namespace = ('kpi-delivery', 'tools', <tool_key>)
name      = checkpoint
```

Payload:

```text
watermark_utc
registry_revision
registry_digest
```

`registry_digest` es guard de integridad, no dimensión normal de versionado.

Pending si:

```text
no checkpoint
OR checkpoint watermark != current committed watermark
OR checkpoint registry_revision != frozen registry revision
```

Misma `registry_revision` con digest distinto es error.
Watermark regression es error.

## Partial failure

Si A, B y C están pendientes y B falla:

```text
A → publish + checkpoint
B → failure; checkpoint no avanza
C → publish + checkpoint
iteration → error global después de procesar todos
```

Siguiente iteración:

```text
A → current / skip
B → retry
C → current / skip
```

## Output owned

```text
container       = ada-kpi-latest-delivery
partition path  = /partition_id
TTL             = None
id              = latest
partition_id    = kpis
document_type   = ada_kpi_latest_delivery
schema_version  = 1
```

Cada Tool publica a su named Cosmos connection.

## Qualification observada

```text
kpi-delivery-runtime    28 passed
Ruff                    PASS
format                  PASS
productive/commented    PASS
public import           PASS
wheel build             PASS
git diff --check        PASS
```

El descenso desde 29 a 28 tests fue intencional: se eliminó un test que congelaba ausencia de terminología/implementación interna.

## UNVERIFIED

```text
real multi-Tool Cosmos run
Azure production credentials
RU profile under representative Tool count
failure/recovery against real Cosmos throttling/network faults
```
