# Manager — Source Blob Handoff

Estado: **SOURCE IMPLEMENTED / MANAGER GENERIC HANDOFF CLOSED / NAVIGATION ADOPTION CLOSED**

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

No convierte identidad Source/Projection a contratos legacy de revisión textual.

## Navigation adoption

Navigation quedó cerrado sobre el handoff genérico.

Local:

```text
LocalSourceStore
 -> NavigationSourceService
 -> generic Manager Source workflows

LocalNavigationProjectionStore
 <- SourceProjectionService
```

Azure:

```text
BlobSourceStore
 -> NavigationSourceService
 -> generic Manager Source workflows

CosmosNavigationProjectionStore
 <- SourceProjectionService
```

No existen adapters Navigation para conservar los stores legacy eliminados.

Estado:

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Consumers

El handoff genérico de Manager está cerrado.

Estado observado:

```text
Navigation        CLOSED / VERIFIED / CURRENT
Users Manager     ALIGNMENT VALIDATION PLANNED / NEXT
Tools             PLANNED
KPI Configuration PLANNED
KPI Definition    PLANNED
```

Users no se considera un consumer cerrado ni un cutover decidido: sólo existe una desalineación verificada que debe adjudicarse.

## Qualification caveat

Current checkpoint:

```text
d34cda3838a67907728b382e238f0178f9f1a64e
```

Navigation + Manager scoped:

```text
102 passed
```

No se afirma:

- full Web GREEN;
- full ADA GREEN;
- Docker E2E;
- Python 3.14.7 qualification global.
