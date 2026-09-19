# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / GENERIC CONSUMER CUTOVER CLOSED**

## Flujo conceptual

```text
WORKSPACE
→ validate
→ verify Source
→ publish Source
→ project
→ history/preview
```

Guardar WORKSPACE no equivale a publicar Source.

## BASE / SOURCE / WORKSPACE / PROJECTION

```text
BASE
    SourceSnapshot observado al establecer/rebasar el workspace

SOURCE
    current durable autoritativo, independiente del workspace

WORKSPACE
    payload editable local + revision local + BASE

PROJECTION
    active projection de una Source release exacta
```

## ManagerModule CURRENT

```text
source_key
source_service
source_reader_service
projection_service
draft_validation_service
source_history_service | None
access_key | None
```

No existe routing alternativo exact/legacy.

## Authorization del workflow

El Manager CURRENT usa una capacidad funcional del módulo:

```text
ManagerAuthorizationPolicy.can_view(principal, module)
```

El coordinator exige esa capacidad antes de todas las operaciones del módulo.

No existen gates separados Manager de:

```text
can_validate
can_publish
can_project
```

No conceden authority implícita:

```text
is_local
administrator profile
```

## Source contracts

```text
SourceReaderWorkflow
SourcePublicationWorkflow
SourceHistoryWorkflow
```

Invariantes:

- Source identity no se reduce a revision strings;
- publication conserva `SourceSnapshot`;
- conflicto se determina por release identity;
- History conserva `SourceReleaseRef`;
- History read devuelve la release solicitada.

## Projection contract

Manager transporta `ProjectionTarget` completo.

- Manager no reconstruye target desde revision;
- target conserva `SourceKey`, release exacta y dependencies;
- target de otro source_key es inválido;
- retry no cambia silenciosamente target.

## Workspace

`ManagerWorkspace` mantiene identidad local del payload separada de Source identity.

ADA Configuration Manager CURRENT usa `ManagerWorkspaceBridge`.

## Configuration Manager adoption

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT
```

CURRENT compone workflows Source/Draft Validation para:

```text
Navigation
Tools
KPI Configuration
KPI Definition
```

Users no usa Manager Source/Projection y no es ManagerModule CURRENT.

## Runtime local CURRENT

Para smoke/manual validation:

```text
LocalSourceStore
InProcessProjectionStore
```

La composition local CURRENT incluye Sources/Projection de:

```text
Navigation
Tools
KPI Configuration
KPI Definition
```

Principal local:

```text
is_local=True
access_keys=(navigation.manage, tools.manage, kpis.manage)
```

`is_local` no concede permiso por sí mismo.

## Callback active workflow

En una transición de ruta, si el módulo no es resoluble/visible mientras Dash conserva
outputs pattern montados, `refresh_active_workflow` ejecuta `PreventUpdate`.

Esto evita cardinalidad inválida.

## Contrato removido

```text
ManagerModuleAccess
ConfigurationLifecycleWorkflow
ExactSourceReaderWorkflow
ExactSourcePublicationWorkflow
ExactSourceHistoryWorkflow
ExactProjectionWorkflow
workflow_service
exact_source_*
exact_projection_service
expected_source_revision
revision -> ProjectionTarget reconstruction
```

## Finding consumer standalone

`web/compositions/navigation-manager` CURRENT invoca `authorization.can_access(...)` y debe
alinearse a `can_view(...)` cuando se trabaje esa composition.

No crear alias de compatibilidad.

## Siguiente frontera

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
PLANNED / NEXT
```

No inventar nueva familia de workflows para recuperar UI.
