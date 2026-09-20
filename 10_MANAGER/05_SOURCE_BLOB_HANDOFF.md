# Manager — Source Blob Handoff

Estado: **SOURCE IMPLEMENTED / MANAGER GENERIC HANDOFF CLOSED / CURRENT CONSUMERS ALIGNED**

## Source productivo

Source productivo objetivo:

```text
Azure Blob Storage
```

Local conserva semántica equivalente de desarrollo/QA.

SharePoint/Power Automate pueden permanecer sólo donde consumers todavía no hayan migrado;
no son autoridad donde Blob ya lo sea.

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
    +
    dependencies exactas cuando correspondan
```

`project(target)` ejecuta el target exacto.

## Manager generic adoption

`ManagerModule` CURRENT expone/consume los workflows genéricos Source/Projection.

No convierte identidad Source/Projection a contratos legacy de revisión textual.

`ManagerEntry` no participa de este handoff y no debe recibir Source/Projection ficticios.

## Consumers Source/Projection CURRENT

```text
Profiles          CLOSED / VERIFIED / CURRENT
Navigation        CLOSED / VERIFIED / CURRENT
Tools             CLOSED / VERIFIED / CURRENT
KPI Configuration CLOSED / VERIFIED / CURRENT
KPI Definition    CLOSED / VERIFIED / CURRENT
ADA Access        SOURCE/PROJECTION CURRENT
```

Profiles se integra mediante:

```text
web/compositions/profiles-manager
```

y su `ManagerModule` ya forma parte de ADA Configuration Manager.

## Users boundary

Users no pertenece al handoff Source/Projection.

```text
Users Administration
SEPARATE ADMIN LIFECYCLE
```

Su integración Manager ya es CURRENT mediante:

```text
web/compositions/users-manager
→ ManagerEntry
```

Estado:

```text
USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT
```

No crear Source/Projection ficticios para Users.

## ADA Access boundary

ADA Access sí posee Source/Projection reales.

CURRENT incluye:

```text
AdaAccessSourceService
AdaAccessProjectionBuilder
ProjectionRecord[AdaAccessConfiguration]
projection-local
projection-cosmos
exact Profiles ProjectionTarget dependency
```

Su superficie administrativa Manager no existe todavía.

```text
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST
```

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

No existen adapters Navigation para conservar stores legacy eliminados.

Estado:

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Checkpoint CURRENT

```text
783d3578da52aeb5cf831999a7717dc8b79f2fb0
```

Parent:

```text
e0dca2d9f9e8db9551b8cee45a37cd1ce3dd4bd5
```

Tree:

```text
5ed091d5477b8ca041ddd669de8217028ec72f35
```

El parent contiene la implementación Users Manager.
El checkpoint CURRENT elimina únicamente el lockfile anidado accidental de esa composition.

## Qualification relevante observada antes del cleanup

```text
Users core tests                    46 passed
Manager tests                       62 passed
users-manager tests                  1 passed
ADA Configuration Manager tests    26 passed

Web scoped combined                 109 passed
ADA Configuration Manager           26 passed
git diff --check                    PASS
```

El cleanup posterior no modifica código ni contracts.

No se afirma:

- full monorepo pytest GREEN;
- full ADA GREEN;
- full Ruff workspace GREEN;
- Docker E2E;
- CI remote GREEN.

## Siguiente frontera administrativa

```text
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST
```

No mezclar con ADA Access runtime composition.
