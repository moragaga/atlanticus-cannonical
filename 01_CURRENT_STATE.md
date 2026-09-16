# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

Corte de implementación verificado para este cierre:

```text
moragaga/atlanticus@d34cda3838a67907728b382e238f0178f9f1a64e
parent: 59fcd3ecc8f3441e64fbe0fc892b4467fa56f181
```

Este documento actualiza únicamente el estado cambiado o revalidado durante el cierre de Navigation. Los dominios no inspeccionados conservan su estado canónico anterior y no se consideran revalidados por este checkpoint.

## Resumen del hito

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER          CLOSED / VERIFIED / CURRENT
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER          CLOSED / VERIFIED / CURRENT

NAVIGATION-MANAGER-GENERIC-CONSUMER-CUTOVER       SUPERSEDED BY CLOSED NAVIGATION CUTOVER NAME

USERS-MANAGER-ALIGNMENT-VALIDATION                 PLANNED / NEXT

TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER             PLANNED
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER       PLANNED
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER   PLANNED

MANAGER-CONSUMER-GLOBAL-QUALIFICATION              BLOCKED
```

## Manager CURRENT

El contrato genérico de Manager permanece congelado:

```text
ManagerModule
├── source_key
├── source_service
├── source_reader_service
├── projection_service
├── draft_validation_service
└── source_history_service | None
```

No reintroducir:

```text
workflow_service
exact_source_reader_service
exact_source_history_service
exact_source_workflow_service
exact_projection_service
```

## Navigation CURRENT

Navigation quedó alineado directamente al contrato genérico.

Composition CURRENT:

```text
web/compositions/navigation-manager
```

Source:

```text
Local
LocalSourceStore
    -> NavigationSourceService
    -> generic Manager Source workflows

Azure
BlobSourceStore
    -> NavigationSourceService
    -> generic Manager Source workflows
```

Projection:

```text
Local
LocalNavigationProjectionStore

Azure
CosmosNavigationProjectionStore
```

Manager services registrados:

```text
navigation.configuration.source
navigation.configuration.projection
navigation.configuration.validation
```

Un solo Source workflow atiende reader/publication/history.

No existe una arquitectura especial de Manager para Navigation.

## Navigation legacy removido

Removida la arquitectura paralela de `web/capabilities/navigation/configuration` basada en:

```text
bundle.py
contracts.py
projection.py
requirements.py
services.py
adapters/
```

También fueron removidos sus mirrors y tests legacy asociados.

No deben reintroducirse:

```text
expected_source_revision
base_source_revision
SOURCE_REVISION_STORE_ID
NavigationConfigurationServices
NavigationAdministrationService
NavigationProjectionWorkflow
compose_navigation_configuration_services
NavigationConfigurationBundle
NavigationConfigurationSourceDocument
NavigationProjectionRepository
NavigationConfigurationPublisher
NavigationConfigurationSource
```

`NavigationConfigurationSourceError` NO es legacy; permanece como error vigente de la frontera Source de Navigation.

## Qualification de Navigation

Ejecutado por el usuario sobre Python `3.14.2` después del cutover:

```text
ruff scoped
PASS

pytest scoped
102 passed
0 failed

forbidden legacy scan
0 results

git diff --check
PASS

git diff --cached --check
PASS
```

`uv lock` resolvió el workspace actualizado e incorporó:

```text
atlanticus-web-composition-navigation-manager 0.1.0
atlanticus-web-navigation-configuration 0.1.9
atlanticus-web-navigation-projection-cosmos 0.1.0
atlanticus-web-navigation-projection-local 0.1.0
```

## Global suite

La ejecución global:

```text
cd web
uv run pytest
```

no llegó a ejecutar la suite por errores de collection en `web/compositions/users-manager`.

Primer error observado:

```text
ImportError:
cannot import name 'ExactSourceHistoryReadResult'
from 'atlanticus.web.manager'
```

El mismo origen bloqueó cuatro módulos de tests de `users-manager`.

## Adjudicación del bloqueo Users

VERIFIED:

- el problema no fue introducido por el commit de Navigation;
- el commit `d34cda3...` sólo cambia el frente de Navigation respecto de su parent;
- `users-manager` conserva archivos `exact_history.py`, `exact_source.py`, `exact_projection.py` y `workspace.py`;
- esos archivos consumen nombres `Exact*` que el Manager CURRENT ya no expone.

Todavía NO está decidido:

- si todos esos archivos deben eliminarse;
- cuál es el corte exacto de implementación;
- si existe documentación canónica específica de Users que refine el contrato;
- qué tests deben reemplazarse versus conservarse.

Por tanto:

```text
USERS-MANAGER-ALIGNMENT-VALIDATION
PLANNED / NEXT
```

No convertirlo todavía en un cutover implementativo.

## OPEN relevante

1. validar la desalineación de `users-manager` contra `atlanticus:main`;
2. contrastarla con `atlanticus-cannonical:main`;
3. identificar contrato final y archivos exactos;
4. sólo después decidir si existe un incremento de implementación;
5. Tools, KPI Configuration y KPI Definition permanecen fuera del próximo chat.

## UNVERIFIED

- full Web suite GREEN en `d34cda3...`;
- full ADA suite;
- Docker E2E;
- CI remoto adicional;
- qualification global Python `3.14.7` / Trixie;
- consumers Tools/KPI Configuration/KPI Definition;
- auditoría exhaustiva de `atlanticus-decisions`.

## Siguiente frontera

```text
NEXT
USERS-MANAGER-ALIGNMENT-VALIDATION
```

Usar obligatoriamente:

```text
moragaga/atlanticus:main
moragaga/atlanticus-cannonical:main
```

No reabrir Navigation.
