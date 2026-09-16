# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada publicada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.
- Git permanece SOLO LECTURA para el asistente.

## Checkpoint publicado de este cierre

```text
moragaga/atlanticus@ee9a0401c7947f2bf61abc0a783dfa905443b6b1
```

Parent inmediato:

```text
ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
```

## Manager core

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Contrato:

```text
ManagerModule
source_key
source_service
source_reader_service
projection_service
draft_validation_service
source_history_service | None
```

## Configuration domains

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## ADA Configuration Manager final consumer

Publicado en:

```text
ee9a0401c7947f2bf61abc0a783dfa905443b6b1
```

Estado:

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Verificado por inspección:

```text
ConfigurationManagerDependencies usa contracts CURRENT
composition.py registra source/reader/history/projection/validation
Users usa users-manager CURRENT
Navigation usa NavigationSourceService + Projection service
Tools usa ToolSourceService + Projection service
KPI usa KpiSourceService + Projection service
KPI Definition usa KpiDefinitionSourceService + Projection service
ManagerWorkspaceBridge presente
local_runtime.py presente
__main__.py presente
```

Removido del consumer:

```text
kpi_authority.py
KpiDefinitionAuthorityProvider
ToolLifecycleServices
KpiConfigurationServices
KpiDefinitionServices
NavigationConfigurationServices
ExactProjectionWorkflow
workflow_service
exact_source_*
expected_source_revision
revision-string projection adapters
```

## Evidencia observada

Antes de publicación:

```text
git diff --check
PASS

legacy token scan scoped
0 matches

compileall scoped
PASS
```

Después:

```text
Configuration Manager local page boot
PASS / manual observation
```

## Qualification pendiente

```text
full Ruff
UNVERIFIED

full pytest
UNVERIFIED

full ADA regression
UNVERIFIED

local behavioral E2E
UNVERIFIED

Storage/Cosmos Docker E2E
UNVERIFIED

CI remote
UNVERIFIED
```

## Python metadata

Configuration Manager CURRENT:

```text
requires-python = "==3.14.2"
```

Canonical baseline:

```text
Python 3.14.7
```

Estado:

```text
OPEN
```

## Próxima frontera

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
PLANNED / NEXT
```

No mezclar E2E, Storage/Cosmos Docker ni Python metadata en ese incremento salvo bloqueo directo.
