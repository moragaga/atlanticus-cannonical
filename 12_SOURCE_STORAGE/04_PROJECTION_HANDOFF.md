# Source Storage — Projection Handoff

Estado: **CLOSED / VERIFIED / CURRENT**

Projection trabaja sobre una Source release concreta.

No lee un `latest` mutable durante ejecución.

## Target exacto

```text
ProjectionTarget =
    SourceKey
    +
    SourceReleaseRef
    +
    dependencies
```

`SourceReleaseId` identifica publicación y no equivale a `content_hash`.

## Selección vs ejecución

Selección puede observar Source current.

Ejecución:

```text
project(target)
```

resuelve exactamente la release del target.

## Provenance durable

Projection activa conserva:

```text
source_key
source_release_id
source_published_at_utc
projected_at_utc
dependencies
payload
```

CURRENT/OUTDATED compara identidad exacta, nunca content hash como sustituto.

## Tool Projection CURRENT

Tool Configuration ahora materializa `ProjectionRecord[ToolConfiguration]` durable.

Codec:

```text
tool_projection_to_document
tool_projection_from_document
```

Providers:

```text
LocalToolProjectionStore
CosmosToolProjectionStore
```

Local:

```text
<base>/<application>/<tool>/projections
```

Cosmos:

```text
partition_key = <application>/<tool>
```

El deployment namespace no modifica `ProjectionTarget` ni `SourceKey`.

## Runtime handoff

La lectura runtime se separa de la materialización:

```text
resolve_active_tool_projection()
→ ProjectionStore.get_active()
→ does not require Source
```

Materialización:

```text
project_current_tool_source()
→ select_current_target()
→ project(target)
```

Esto evita usar Source availability como requisito para consumir una Projection durable ya
existente.

## Resolution states

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

No equivalen a estados globales del proceso Web.

## Contratos superseded

No reintroducir:

```text
ExactProjectionWorkflow
ConfigurationLifecycleWorkflow
expected_source_revision
projection revision textual
revision -> ProjectionTarget reconstruction
in-process Projection as durable authority
```

## Qualification actual

Checkpoint CURRENT:

```text
21cfb2f11362c1606ad14ff8adc7551948eced6a
```

Tool persistence composition:

```text
10 passed
ruff check PASS
ruff format --check PASS
git diff --check PASS
```

## Fuera de este cierre

El consumer ADA Generic todavía no usa esta composición en startup.

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

No reabrir Projection Core para resolver ese consumer.
