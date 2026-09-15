# Manager — Source Blob Handoff

Estado: **SOURCE IMPLEMENTED / MANAGER GENERIC HANDOFF CLOSED / CONSUMER MIGRATION IN PROGRESS**

## Source productivo

Source productivo objetivo:

```text
Azure Blob Storage
```

Local conserva semántica equivalente de desarrollo/QA.

SharePoint/Power Automate pueden permanecer sólo donde consumidores todavía no hayan migrado; no son autoridad donde Blob ya lo sea.

## Source Core

Source Core cubre:

- `SourceKey`;
- `SourceReleaseId`;
- `SourceReleaseRef`;
- immutable releases;
- manifest;
- `SourceStore`;
- current/concurrency;
- History;
- exact reads;
- integrity verification.

## History

Cada publicación Source es snapshot completo, autocontenido e inmutable.

`SourceReleaseId` no equivale a `content_hash`.

History durable contiene publicaciones reales, no autosaves.

## Restore

Restore publica una nueva release.

Nunca repunta current directamente a una release histórica.

## Concurrencia

Backend aplica la precondición autoritativa con `ConcurrencyToken`.

Manager conserva `SourceSnapshot` hasta publication y relee current antes de invocar el workflow.

## Source -> Projection

```text
ProjectionTarget =
    SourceKey
    +
    SourceReleaseRef
```

`project(target)` ejecuta el target exacto.

## Manager generic adoption

Manager CURRENT expone:

```text
SourceReaderWorkflow
SourcePublicationWorkflow
SourceHistoryWorkflow
```

y consume Projection mediante:

```text
get_status(source_key)
select_current_target(source_key)
project(ProjectionTarget)
```

No convierte `SourceReleaseRef`, `ConcurrencyToken`, `SourceSnapshot`, `HistoryPage`, `ProjectionTarget` ni `ProjectionExecutionResult` a contratos legacy de revisión textual.

## Cutover

Removido del contrato Manager:

```text
ExactSourceReaderWorkflow
ExactSourcePublicationWorkflow
ExactSourceHistoryWorkflow
ExactProjectionWorkflow
ConfigurationLifecycleWorkflow
expected_source_revision
```

No existe dual contract.

## Consumers

El handoff genérico de Manager está cerrado.

La adopción de cada módulo consumidor permanece separada:

```text
Navigation        PLANNED / NEXT
Tools             PLANNED
KPI Configuration PLANNED
KPI Definition    PLANNED
```

No crear adapters para mantener el API Manager anterior.

## Qualification caveat

Current checkpoint:

```text
59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
```

Manager scoped suite:

```text
54 passed
```

No se afirma:

- full Web GREEN;
- full ADA GREEN;
- consumer suites GREEN;
- Docker E2E;
- Python 3.14.7 qualification global.
