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

## Provenance durable

Projection activa conserva identidad de Source release exacta.

CURRENT/OUTDATED compara release identity, nunca content hash.

## Manager generic handoff

Manager consume directamente Projection Core.

Contrato de servicio esperado por el coordinator:

```text
get_status(source_key) -> ProjectionStatus
select_current_target(source_key) -> ProjectionTarget | None
project(target) -> ProjectionExecutionResult
```

Invariantes:

- el target llega completo a `project`;
- el target debe usar el `SourceKey` del módulo;
- Manager no reconstruye target desde revision;
- Manager no crea un resultado paralelo;
- `ProjectionExecutionResult.target` conserva el target ejecutado.

## Contrato superseded

Ya no forman parte de la frontera Manager:

```text
ExactProjectionWorkflow
ConfigurationLifecycleWorkflow
get_current_projection_target() sin source_key como servicio Manager específico
project(expected_source_revision)
projection revision textual
```

La semántica exact-release permanece; lo que se elimina es la duplicación Manager `exact` vs `legacy`.

## Source publication handoff

Manager publication usa `SourceSnapshot`.

No usa:

```text
expected_source_revision
```

La selección del target posterior se realiza desde el servicio Projection con el `SourceKey`.

## Qualification actual

Current implementation checkpoint:

```text
59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
```

Manager scoped suite:

```text
54 passed
```

## Fuera de este cierre

OPEN:

- Navigation consumer alignment;
- Tools consumer alignment;
- KPI Configuration consumer alignment;
- KPI Definition consumer alignment;
- global consumer qualification;
- demás contratos especializados ya abiertos.

No reabrir Projection Core para resolver un consumer.
