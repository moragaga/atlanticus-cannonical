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
moragaga/atlanticus@4c7f8aa8b541e8b8f8abc7b49fe22526a4952bfe
```

Parent inmediato:

```text
27c2e4beed125fe379881048f0df5fbe3ff6cb1a
```

Ese checkpoint contiene el clean cutover de KPI Configuration hacia Source/Projection genéricos.

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
PLANNED / NEXT

ADA-CONFIGURATION-MANAGER-FINAL-CUTOVER
BLOCKED

MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED
```

## KPI Configuration clean cutover

KPI Configuration permanece ADA-specific:

```text
scopes/ada/web/kpis/configuration
```

Usar Source/Projection genéricos no cambia su ownership ni convierte el scope ADA en core Atlanticus.

Contrato CURRENT:

```text
KpiSourceService
SourceStore
SourceSnapshot
SourceReleaseRef
PublishRequest / PublishResult
KpiProjectionBuilder
ProjectionTarget
ProjectionStore[KpiConfiguration]
SourceProjectionService[KpiConfiguration]
KpiDestinationCatalogSnapshot
```

La KPI Configuration Projection depende del `ProjectionTarget` exacto de Tool Projection.

Removido del contrato CURRENT:

```text
private Source/Projection lifecycle
private source revision identity
private projection revision identity
tool_projection_revision as dependency identity
expected_source_revision
revision -> ProjectionTarget reconstruction
compatibility adapters / shims / aliases
```

## Qualification observada

Para el cutover KPI Configuration:

```text
uv lock
PASS

uv sync --group dev
PASS

uv run ruff check src tests
PASS

uv run pytest
45 passed

git diff --check
PASS

legacy token scan over src/commented/tests
0 matches after generated build/cache cleanup
```

El checkpoint publicado fue inspeccionado después de esa qualification local.

CI remoto, full ADA regression y Docker E2E permanecen UNVERIFIED.

## Conflicto abierto de baseline Python

La decisión canónica global permanece:

```text
Python 3.14.7
```

El `pyproject.toml` publicado de KPI Configuration todavía declara:

```text
requires-python = "==3.14.2"
```

No se corrige dentro de este cierre ni se mezcla con KPI Definition.

## Siguiente foco único

```text
KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

Primero inspección y diseño sobre la implementación CURRENT. No tocar todavía el Configuration Manager final, Command Center, Operational Data ni otros frentes.

Atajos:

- Manager → `10_MANAGER/00_INDEX.md`
- ADA Configuration → Runtime → `11_ADA_GENERIC/03_CONFIGURATION_TO_RUNTIME.md`
- Validation → `07_VALIDATION_BASELINE.md`
- Roadmap → `08_ROADMAP.md`
- Open items → `09_OPEN_QUESTIONS.md`
