# Manager — Workflow and Session

Estado: **CURRENT CONTRACT / GENERIC CONSUMER CUTOVER CLOSED / PROFILES + USERS INTEGRATED**

## Flujo conceptual de ManagerModule

```text
WORKSPACE
→ validate
→ verify Source
→ publish Source
→ project
→ history/preview
```

Guardar WORKSPACE no equivale a publicar Source.

Este flujo aplica a `ManagerModule`.

No aplica automáticamente a `ManagerEntry`.

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

## ManagerEntry CURRENT

```text
key
group_key
title
route
order
layout
description
access_key | None
web_module | None
```

`ManagerEntry` comparte shell, routing, authorization y WebModule lifecycle.

No requiere:

```text
source_key
source_service
projection_service
workspace Source
projection status
```

No fabricar esos contratos para capabilities administrativas que no los poseen.

## Authorization

Manager CURRENT usa:

```text
ManagerAuthorizationPolicy.can_view(principal, item)
```

para:

```text
ManagerModule | ManagerEntry
```

En `ManagerModule`, el coordinator exige capacidad antes de las operaciones del módulo.

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

Users conserva su lifecycle administrativo propio y entra al shell mediante
`ManagerEntry`.

## Configuration Manager adoption

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-ADA-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT
```

CURRENT compone:

```text
Administración:
- Users

Configuraciones:
- Profiles
- Navigation
- Tools
- KPI Configuration
- KPI Definition
```

Profiles llega desde:

```text
web/compositions/profiles-manager
```

Users llega desde:

```text
web/compositions/users-manager
```

El `ManagerModule.web_module` de Profiles registra workflows Source/Projection/Validation.

El `ManagerEntry.web_module` de Users registra el servicio administrativo existente y la
Web surface Users.

No existe registry auxiliar ni adapter de integración.

## Runtime local CURRENT

Principal local:

```text
is_local=True
access_keys=(
    users.manage,
    profiles.manage,
    navigation.manage,
    tools.manage,
    kpis.manage,
)
```

`is_local` no concede permiso por sí mismo.

Users local usa stores in-process para smoke/runtime local y recibe un
`UsersAdministrationService` compuesto explícitamente.

Las opciones de Profiles consumidas por Users provienen del Profiles projection store
inyectado en el runtime local.

## Users UI lifecycle CURRENT

La Web surface Users:

- obtiene un snapshot inicial con `UsersAdministrationService.discover()`;
- permite candidate/promote y promoted/edit;
- sólo administra `profile_key` y `enabled`;
- mantiene identidad/directory fields read-only;
- muestra conflictos en vez de corregirlos silenciosamente;
- persiste sólo mediante `UsersAdministrationService`;
- usa paginación 10/20;
- conserva el snapshot de opciones Profiles durante promote/update;
- sólo adopta un nuevo snapshot de Profiles mediante Refresh explícito.

No introducir auto-refresh de Profiles ni validación de borrado/orphan fuera de un
incremento dedicado.

## Callback active workflow

En una transición de ruta, si un `ManagerModule` no es resoluble/visible mientras Dash
conserva outputs pattern montados, `refresh_active_workflow` ejecuta `PreventUpdate`.

Los callbacks Source/Projection ignoran `ManagerEntry`.

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

`web/compositions/navigation-manager` CURRENT invoca un consumer de authorization
desalineado respecto de `can_view(...)`.

No crear alias de compatibilidad.

## Siguiente frontera

```text
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST
```

ADA Access ya posee Source/Projection y persistencia CURRENT.

No existe Web surface de Access CURRENT.

El siguiente diseño debe inspeccionar primero sus contracts existentes y sus consumers antes
de definir la superficie administrativa, el catálogo/definición de accesos o el identificador
estable que consumirá manualmente el desarrollador.

Después:

```text
MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED
```
