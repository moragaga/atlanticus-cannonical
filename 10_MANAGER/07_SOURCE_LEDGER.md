# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada publicada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.
- Git permanece SOLO LECTURA para el asistente.

## Checkpoint publicado de este cierre

```text
moragaga/atlanticus@ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
```

Parent inmediato:

```text
4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
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

## Navigation

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Users

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT
```

## Tools Source/Projection

```text
TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## KPI Configuration Source/Projection

Publicado en:

```text
4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
```

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Qualification scoped documentada:

```text
Ruff PASS
pytest 45 passed
git diff --check PASS
legacy token scan 0 matches
```

## KPI Definition Source/Projection

Publicado en:

```text
ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
```

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

CURRENT:

```text
KpiDefinitionSourceService
KpiDefinitionProjectionBuilder
KpiDefinitionCatalog
exact KPI Configuration ProjectionTarget dependency
```

Removido del dominio:

```text
KpiDefinitionAuthorityCatalog
KpiDefinitionAuthorityProvider
KpiDefinitionServices
private revision lifecycle
expected_source_revision
build_kpi_definition_digest as identity
```

Qualification scoped documentada:

```text
Python shell 3.14.7
uv lock PASS
uv sync --group dev --extra web PASS
Ruff PASS
pytest 40 passed
legacy token scan 0 matches
```

## Desalineación CURRENT del consumer

`ada-configuration-manager` todavía contiene imports y adapters del contrato anterior:

```text
ToolLifecycleServices
KpiConfigurationServices
KpiDefinitionServices
KpiDefinitionAuthorityProvider
ExactProjectionWorkflow
workflow_service
exact_source_*
expected_source_revision
revision-string workflow adapters
```

También importa nombres `create_users_manager_exact_source_*` que ya no forman parte de Users Manager CURRENT.

No es motivo para reintroducir legacy en los dominios.

## Regla

```text
DOMAIN FINAL CONTRACT FIRST
CONSUMER CUTOVER LAST
NO TEMPORARY COMPATIBILITY
```

Todos los dominios Configuration requeridos ya alcanzaron contrato final.

## Qualification pendiente

```text
ADA Configuration Manager final runtime
UNVERIFIED

full ADA regression after consumer cutover
BLOCKED

CI remote status for ef3f0a44...
UNVERIFIED
```

## Próxima frontera

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

No mezclar cleanup transversal de tests Web ni Python metadata en ese incremento salvo bloqueo real.
