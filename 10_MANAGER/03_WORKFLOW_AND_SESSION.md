# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / GENERIC CONSUMER CUTOVER CLOSED / PROFILES INTEGRATED**

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

ADA Configuration Manager CURRENT usa `ManagerWorkspaceBridge` para los consumers que ya
están implementados de esa manera.

Profiles conserva su propia composition reusable y no duplica ese wiring dentro de ADA.

## Configuration Manager adoption

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-ADA-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT
```

CURRENT compone:

```text
Profiles
Navigation
Tools
KPI Configuration
KPI Definition
```

Profiles llega desde:

```text
web/compositions/profiles-manager
```

El `ManagerModule.web_module` de Profiles registra los workflows Source/Projection/Validation
en el `ServiceRegistry` real de la aplicación.

No existe registry auxiliar ni adapter de integración.

Users no usa Manager Source/Projection y no es `ManagerModule` CURRENT.

## Runtime local CURRENT

Para smoke/manual validation de configuration:

```text
LocalSourceStore
InProcessProjectionStore
```

La composition local CURRENT incluye:

```text
Profiles
Navigation
Tools
KPI Configuration
KPI Definition
```

Principal local:

```text
is_local=True
access_keys=(
    profiles.manage,
    navigation.manage,
    tools.manage,
    kpis.manage,
)
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

No reintroducirlos.

## Finding consumer standalone

`web/compositions/navigation-manager` CURRENT invoca `authorization.can_access(...)` y debe
alinearse a `can_view(...)` cuando ese consumer entre al scope.

No crear alias de compatibilidad.

## Siguiente frontera

```text
USERS-ADMINISTRATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST
```

Users ya posee `UsersAdministrationService`.

La etapa siguiente debe revisar y reutilizar ese lifecycle.
No crear Source/Projection ficticios, adapters ni un contrato administrativo paralelo.

Después:

```text
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED

MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED
```
