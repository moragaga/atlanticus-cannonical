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
```

`SourceReleaseRef` conserva referencia resoluble exacta.

`SourceReleaseId` identifica publicación y no equivale a `content_hash`.

## Selección vs ejecución

Selección puede observar Source current.

Ejecución:

```text
project(target)
```

resuelve exactamente la release del target y no relee current para sustituirla.

Source puede avanzar durante ejecución sin invalidar una Projection exacta ya iniciada.

## Provenance durable

Projection activa conserva:

- `source_key`;
- `source_release_id`;
- `source_published_at_utc`;
- `projected_at_utc`.

CURRENT/OUTDATED compara release identity, nunca content hash.

## Alignment

```text
NEVER_PROJECTED
CURRENT
OUTDATED
```

Attempt outcome:

```text
SUCCESS
FAILED
```

Failure no reemplaza la última Projection exitosa.

## Projection Store Core

Core expone:

```text
get_active(source_key)
replace_active(projection)
```

No define history genérico ni failure journal durable.

## Users CURRENT payload

La evidencia temprana de Users/Cosmos usó un aggregate anterior.

Ese payload fue SUPERSEDED por UCS-1.

Contrato CURRENT:

```text
ProjectionStore[UsersProfilesConfiguration]
ProjectionRecord[UsersProfilesConfiguration]
```

`CosmosUsersConfigurationProjectionStore`:

- escribe schema `2`;
- puede leer schema `1` histórico;
- usa create-only first write;
- reemplazo por ETag/CAS;
- no blind upsert;
- same exact target + same payload es idempotente;
- target histórico explícito puede activarse;
- no infiere ordering por release id/timestamps.

## Manager root exact-target

CLOSED:

- `get_current_projection_target()`;
- `project(ProjectionTarget)`;
- result conserva exact target;
- callback selecciona current server-side;
- browser no aporta identidad ejecutable.

## Manager exact Projection boundary

Manager además posee:

```text
ExactProjectionWorkflow
    get_status()
    get_current_projection_target()
    project(target)
```

El status es `projection/core.ProjectionStatus`, no un adapter al modelo legacy.

Manager presentation deriva state desde:

```text
source_current_release
projected_source_release
alignment
```

No inventa audit/projection revision.

## Users exact Projection adoption

CURRENT:

```text
UsersManagerExactProjectionWorkflow
    SourceProjectionService[UsersProfilesConfiguration]
    + SourceKey
```

El workflow:

- obtiene exact status;
- selecciona target current;
- valida source key del target;
- proyecta exactamente el target.

ADA recibe esa capability ya compuesta mediante `users_exact_projection`.

## Qualification actual

Current implementation checkpoint:

```text
384a68fe8fa42263623c95d1d132af2ca54574c8
```

Users exact Projection status/host está CLOSED/VERIFIED/CURRENT.

Los failures ADA restantes pertenecen a legacy adapters no-Users y no contradicen el handoff exacto Users.

## Fuera de este cierre

OPEN:

- `users.runtime` exact provenance;
- runtime canonical cutover;
- legacy Projection alignment Navigation/Tools/KPI/KPI Definitions;
- otros providers/domain consumers;
- orchestration multi-capability;
- retention/GC;
- resource topology físico Users Projection;
- browser IndexedDB global.
