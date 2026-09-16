# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@ee9a0401c7947f2bf61abc0a783dfa905443b6b1
```

Parent inmediato:

```text
ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
```

Canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical@c6c49d72638483d5bec3d2cf9745de3745f1c703
```

Git permanece SOLO LECTURA para el asistente.

## Estado resumido

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER          CLOSED / VERIFIED / CURRENT
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER           CLOSED / VERIFIED / CURRENT
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER             CLOSED / VERIFIED / CURRENT
USERS-CLEAN-CUTOVER-COMPLETION                     CLOSED / VERIFIED / CURRENT
USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL        CLOSED / VERIFIED / CURRENT
PROJECTION-CORE-STALE-TEST-ALIGNMENT               CLOSED / VERIFIED
TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER            CLOSED / VERIFIED / CURRENT
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER       CLOSED / VERIFIED / CURRENT
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER   CLOSED / VERIFIED / CURRENT
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER    CLOSED / VERIFIED / CURRENT
ADA-CONFIGURATION-MANAGER-UI-CLEANUP               PLANNED / NEXT
ADA-CONFIGURATION-MANAGER-LOCAL-E2E                 PLANNED / AFTER UI CLEANUP
ADA-CONFIGURATION-MANAGER-STORAGE-COSMOS-E2E        PLANNED / AFTER LOCAL E2E
MANAGER-CONSUMER-GLOBAL-QUALIFICATION               PLANNED / UNBLOCKED
WEB-TEST-CONTRACT-CLEANUP                           PLANNED
```

## VERIFIED

### Published checkpoint

`main` avanzó a `ee9a0401c7947f2bf61abc0a783dfa905443b6b1`, hijo directo de `ef3f0a44c5dcc14f8fcafe5bb36bb97865381924`.

### Configuration Manager final generic cutover

El package publicado:

```text
scopes/ada/web/application/ada-configuration-manager
```

ya consume directamente los contratos CURRENT de Users, Navigation, Tools, KPI Configuration y KPI Definition.

`ConfigurationManagerDependencies` usa tipos finales:

```text
UsersProfilesAdministrationService
SourceProjectionService[UsersProfilesConfiguration]

NavigationSourceService
SourceProjectionService[NavigationConfigurationCatalog]

ToolSourceService
SourceProjectionService[ToolConfiguration]

KpiSourceService
SourceProjectionService[KpiConfiguration]
KpiDestinationCatalogProvider
ProjectionStore[KpiConfiguration]

KpiDefinitionSourceService
SourceProjectionService[KpiDefinitionCatalog]
```

No depende de los bundles legacy removidos.

### Manager module wiring

La composición publicada registra por módulo servicios separados para:

```text
source
source-reader
source-history
projection
draft-validation
```

y construye `ManagerModule` con:

```text
source_key
source_service
source_reader_service
source_history_service
projection_service
draft_validation_service
```

No existe doble routing legacy/exact en el consumer final.

### Workspace

El consumer usa `ManagerWorkspace` como documento de workspace.

`ManagerWorkspaceBridge`:

- lee/escribe payload dentro de `ManagerWorkspace`;
- valida ownership por `owner_subject_id`;
- obtiene `SourceSnapshot` sólo al crear un workspace nuevo;
- no reconstruye Source identity desde revision strings.

La identidad local de workspace sigue separada de Source y Projection.

### Local runtime

El package publicado contiene entrypoint local y composición local.

CURRENT para smoke/manual validation:

```text
LocalSourceStore
InProcessProjectionStore
EmptyPendingUsersReader
Users Source/Projection
Navigation Source/Projection
Tools Source/Projection
KPI Configuration Source/Projection
KPI Definition Source/Projection
```

Las source keys locales son:

```text
users
navigation
tools
kpis
kpi-definitions
```

El principal local es administrador local.

### Legacy removal

Scans observados durante el cierre no encontraron:

```text
ExactProjectionWorkflow
expected_source_revision
KpiDefinitionAuthorityProvider
KpiConfigurationServices
KpiDefinitionServices
ToolLifecycleServices
NavigationConfigurationServices
base_source_revision
build_kpi_configuration_digest
build_kpi_definition_digest
build_tool_configuration_digest
ManagerDraft
tool_projection_revision
```

### Static validation observada

```text
git diff --check
PASS

legacy token scan scoped
0 matches

python compileall scoped
PASS
```

### Runtime smoke observada

El usuario levantó el Configuration Manager local y confirmó que la página carga.

Resultado:

```text
Configuration Manager UI boot
PASS / manual smoke
```

## INFERRED

La recuperación de la UI demuestra que el consumer dejó de estar bloqueado por los contratos legacy que impedían componer el Manager.

No demuestra todavía que cada acción de edición/publicación/proyección funcione de punta a punta.

## ASSUMED

No se asume:

- que todos los faltantes visuales estén identificados;
- que los contratos que el usuario percibió como “raros” estén mal ni cuál es su causa;
- que full Ruff haya pasado en este checkpoint;
- que full pytest haya pasado en este checkpoint;
- que full ADA regression haya pasado;
- que el flujo `edit → validate → publish → project` haya sido ejecutado completo;
- que el runtime local represente la topología productiva final;
- que Storage/Cosmos Docker E2E ya exista o pase;
- que CI remoto pase;
- que Python 3.14.7 esté alineado en metadata de todos los packages.

## PROPOSED

Único foco siguiente:

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
PLANNED / NEXT
```

Alcance:

- observar la UI CURRENT;
- corregir faltantes visuales/funcionales concretos;
- verificar cualquier contrato sospechoso sólo cuando exista evidencia reproducible;
- no rediseñar contratos congelados sin conflicto real.

## SUPERSEDED

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER as PLANNED / NEXT
SUPERSEDED
```

Ahora:

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT
```

También queda SUPERSEDED la afirmación canónica previa de que `ada-configuration-manager` todavía usaba:

```text
KpiConfigurationServices
KpiDefinitionServices
KpiDefinitionAuthorityProvider
ToolLifecycleServices
ExactProjectionWorkflow
NavigationConfigurationServices
workflow_service
exact_source_*
expected_source_revision
revision-string projection adapters
```

Esos hallazgos describen `ef3f0a44...`, no `ee9a0401...`.

## UNVERIFIED / OPEN

- UI cleanup del Manager;
- naturaleza exacta de los “contratos raros” observados manualmente;
- full Ruff/pytest del package después del cutover publicado;
- full ADA regression;
- local E2E de comportamiento;
- Storage/Cosmos Docker E2E;
- CI remoto;
- Python 3.14.7/Trixie global qualification;
- metadata `requires-python` global;
- cleanup transversal posterior de tests Web.

## Conflicto de baseline Python

Canonical fija:

```text
Python 3.14.7
```

El Configuration Manager publicado todavía declara:

```text
requires-python = "==3.14.2"
```

Este conflicto sigue OPEN y no fue resuelto por el cutover.

## Siguiente frontera

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
PLANNED / NEXT
```

No mezclar todavía E2E, Storage/Cosmos Docker, Python baseline cleanup ni otros frentes.
