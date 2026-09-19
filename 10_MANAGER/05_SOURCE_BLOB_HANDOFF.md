# Manager — Source Blob Handoff

Estado: **SOURCE IMPLEMENTED / MANAGER GENERIC HANDOFF CLOSED / CURRENT CONSUMERS ALIGNED**

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

Dependencies exactas forman parte del `ProjectionTarget` cuando el dominio las requiere.

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

## Consumers CURRENT

Estado verificado:

```text
Profiles          CLOSED / VERIFIED / CURRENT
Navigation        CLOSED / VERIFIED / CURRENT
Tools             CLOSED / VERIFIED / CURRENT
KPI Configuration CLOSED / VERIFIED / CURRENT
KPI Definition    CLOSED / VERIFIED / CURRENT
```

Profiles se integra mediante:

```text
web/compositions/profiles-manager
```

y su `ManagerModule` ya forma parte de ADA Configuration Manager.

Users no pertenece a este handoff Source/Projection:

```text
Users Administration
SEPARATE ADMIN LIFECYCLE
```

No crear Source/Projection ficticios para Users.

ADA Access sí posee Source/Projection y su persistencia Projection ya es CURRENT, pero su
superficie administrativa Manager sigue pendiente.

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

## Checkpoint CURRENT de este cierre

```text
415c8263c15bae2b5d3c01b734b0f1e0101a7242
```

Parent:

```text
9b623d67e253413f6b0d894e10d9cc15735b553d
```

Tree:

```text
ff81bcd74b5e844604ca656562f9aaf398946d67
```

Qualification observada en los scopes modificados:

```text
profiles-manager tests            8 passed
ADA Configuration Manager tests  26 passed
Ruff changed-scope                PASS
git diff --check                  PASS
```

No se afirma:

- full Web GREEN;
- full ADA GREEN;
- Docker E2E;
- Python 3.14.7 qualification global;
- CI remote GREEN.

## Siguiente frontera administrativa

```text
USERS-ADMINISTRATION-MANAGER-INTEGRATION
PLANNED / NEXT
```

No es un cutover Source/Projection.
