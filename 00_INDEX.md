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
moragaga/atlanticus@27c2e4beed125fe379881048f0df5fbe3ff6cb1a
```

Ese checkpoint contiene el clean cutover de Tools Configuration hacia Source/Projection genéricos.

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

TOOLS-SCOPED-QUALIFICATION
PLANNED / UNVERIFIED

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED

ADA-CONFIGURATION-MANAGER-FINAL-CUTOVER
BLOCKED

MANAGER-CONSUMER-GLOBAL-QUALIFICATION
BLOCKED
```

## Tools clean cutover

Tools permanece ADA-specific:

```text
scopes/ada/web/tools
```

Removido de Tool Configuration CURRENT:

```text
contracts.py
lifecycle.py
projection.py
services.py
source.py
ToolLifecycle*
ToolConfigurationSourceSnapshot
ToolConfigurationProjectionSnapshot
expected_source_revision / expected_revision domain contracts
private projection revision identity
```

Reemplazo CURRENT:

```text
ToolSourceService
SourceStore
SourceSnapshot
SourceReleaseRef
PublishRequest / PublishResult
ToolProjectionBuilder
ProjectionTarget
ProjectionStore[ToolConfiguration]
SourceProjectionService[ToolConfiguration]
```

El consumer Manager todavía no fue migrado y no debe sostenerse con compatibilidad temporal.

## Regla vigente

```text
LEGACY                          REMOVE
ADAPTERS / SHIMS / ALIASES     FORBIDDEN
DOBLE CONTRATO                  FORBIDDEN
OLD SCHEMAS IN RUNTIME CODE     FORBIDDEN
revision -> ProjectionTarget    REMOVE
expected_source_revision        REMOVE
```

## Qualification

La evidencia de qualification global publicada sigue correspondiendo al cierre anterior de Users.

Para `27c2e4be...`:

```text
Tools contract/code inspection
VERIFIED

Tools scoped test execution
UNVERIFIED

full ADA / final Manager regression
PLANNED AFTER CONFIGURATION CUTOVERS
```

## Siguiente foco único

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
PLANNED / NEXT
```

Usar Tools CURRENT como referencia estructural. No tocar Manager, KPI Definition, Command Center ni Operational Data en el mismo incremento.

Atajos:

- Manager → `10_MANAGER/00_INDEX.md`
- Validation → `07_VALIDATION_BASELINE.md`
- Roadmap → `08_ROADMAP.md`
- Open items → `09_OPEN_QUESTIONS.md`
