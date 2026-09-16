# Atlanticus Canonical Context — Index

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

| Archivo | Contenido | Estado |
|---|---|---|
| `00_AUTHORITY.md` | Autoridad de fuentes y conflictos. | CURRENT |
| `01_CURRENT_STATE.md` | Estado implementado, validado y pendiente. | CURRENT |
| `02_ARCHITECTURE.md` | Fronteras y dependencias. | CURRENT |
| `03_DECISIONS_CURRENT.md` | Decisiones activas y reglas de cutover. | CURRENT |
| `04_ALARM_ENGINE/` | Alarm Engine + qualification/preservation. | CURRENT + PRESERVATION |
| `05_ENGINEERING_BASELINE.md` | Baseline técnica. | CURRENT |
| `06_OPERATING_MODEL.md` | Modelo operativo/deployment. | CURRENT |
| `07_VALIDATION_BASELINE.md` | Evidencia de qualification y adjudicación. | CURRENT |
| `08_ROADMAP.md` | Orden de ejecución desde Baseline 1.0. | CURRENT |
| `09_OPEN_QUESTIONS.md` | Open items vigentes. | CURRENT |
| `10_MANAGER/` | Manager genérico, Source/Projection y consumers administrativos. | CURRENT |
| `11_ADA_GENERIC/` | ADA Generic, ownership ADA y cadena Tool → KPI → runtime. | CURRENT DIRECTION |
| `12_SOURCE_STORAGE/` | Source/Projection exact-release y storage. | IN PROGRESS |
| `13_ADA_WEB/` | ADA Web y management. | CURRENT DIRECTION |
| `14_ADA_COMMAND_CENTER/` | Command Center y Alarm ownership. | CURRENT DIRECTION |
| `15_WEB_PLATFORM/` | Web platform, Activity, startup y projections. | CURRENT |
| `16_KPI_BACKEND_RECOVERY/` | Reprocessing/recovery KPI. | CURRENT DIRECTION |
| `17_DISTRIBUTION_AND_TOOLING/` | Generators, artifacts, scripts, docs y services. | CURRENT DIRECTION |
| `18_UNIVERSITY/` | Casos pedagógicos reales. | CURRENT DIRECTION |
| `ATLANTICUS_ENGINEERING_RULES.md` | Reglas de ingeniería. | CURRENT |
| `BASELINE_CLOSURE.md` | Qué queda congelado y qué no. | CURRENT |

## Autoridad de implementación

```text
moragaga/atlanticus@ef3f0a44c5dcc14f8fcafe5bb36bb97865381924
```

Parent inmediato:

```text
4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
```

Ese checkpoint contiene el clean cutover de KPI Definition hacia Source/Projection genéricos.

## Estado de hitos

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CLEAN-CUTOVER-COMPLETION
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
CLOSED / VERIFIED / CURRENT

PROJECTION-CORE-STALE-TEST-ALIGNMENT
CLOSED / VERIFIED

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT

MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED

WEB-TEST-CONTRACT-CLEANUP
PLANNED / AFTER MANAGER
```

## KPI Definition clean cutover

KPI Definition permanece ADA-specific:

```text
scopes/ada/web/kpis/definition
```

Contrato CURRENT:

```text
KpiDefinitionSourceService
KpiDefinitionSourceCodec
KpiDefinitionSourcePayload
KpiDefinitionSourceRelease
SourceStore
SourceSnapshot
SourceReleaseRef
KpiDefinitionProjectionBuilder
ProjectionTarget
ProjectionStore[KpiDefinitionCatalog]
SourceProjectionService[KpiDefinitionCatalog]
ProjectionStore[KpiConfiguration]
KpiDefinitionCatalog
```

La KPI Definition Projection depende exactamente del `ProjectionTarget` activo de KPI Configuration y exige que ese target siga siendo el mismo al ejecutar la proyección.

Semántica CURRENT:

```text
configured KPI + Definition     -> DEFINED
configured KPI + no Definition  -> MISSING valid coverage
Definition for non-configured KPI -> invalid projection
missing KPI Configuration projection -> projection error
```

Removido del contrato CURRENT:

```text
KpiDefinitionAuthorityCatalog
KpiDefinitionAuthorityProvider
KpiDefinitionServices
private Source/Projection lifecycle
private source revision identity
private projection revision identity
kpi_configuration_revision as dependency identity
expected_source_revision
revision -> ProjectionTarget reconstruction
build_kpi_definition_digest as Source/workspace identity
compatibility adapters / shims / aliases
```

## Qualification observada

Para KPI Definition, antes de publicación:

```text
Python shell
3.14.7

uv lock
PASS

uv sync --group dev --extra web
PASS

uv run ruff check src tests
PASS

uv run pytest
40 passed

legacy token scan scoped over src/commented/tests
0 matches
```

El checkpoint publicado fue inspeccionado después de esa qualification local.

CI remoto, full ADA regression y Docker E2E permanecen UNVERIFIED.

## Conflicto abierto de baseline Python

La decisión canónica global permanece:

```text
Python 3.14.7
```

KPI Configuration y KPI Definition publicados todavía declaran:

```text
requires-python = "==3.14.2"
```

La ejecución scoped de KPI Definition observó Python 3.14.7, pero eso no resuelve la metadata contractual.

## Siguiente foco único

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

Todos los dominios Configuration relevantes ya tienen contrato Source/Projection final. El siguiente incremento debe cortar `ada-configuration-manager` completo al Manager genérico CURRENT en lugar de crear un cutover específico sólo para KPI Definition.

Atajos:

- Manager → `10_MANAGER/00_INDEX.md`
- ADA Configuration → Runtime → `11_ADA_GENERIC/03_CONFIGURATION_TO_RUNTIME.md`
- Validation → `07_VALIDATION_BASELINE.md`
- Roadmap → `08_ROADMAP.md`
- Open items → `09_OPEN_QUESTIONS.md`
