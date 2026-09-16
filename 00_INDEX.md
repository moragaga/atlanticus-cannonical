# Atlanticus Canonical Context — Index

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

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

`moragaga/atlanticus-decisions` permanece HISTORICAL.

Git permanece SOLO LECTURA para el asistente.

## Índice

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
| `10_MANAGER/` | Manager genérico, Configuration Manager y consumers administrativos. | CURRENT |
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
CLOSED / VERIFIED / CURRENT

ADA-CONFIGURATION-MANAGER-UI-CLEANUP
PLANNED / NEXT

ADA-CONFIGURATION-MANAGER-LOCAL-E2E
PLANNED / AFTER UI CLEANUP

ADA-CONFIGURATION-MANAGER-STORAGE-COSMOS-E2E
PLANNED / AFTER LOCAL E2E

MANAGER-CONSUMER-GLOBAL-QUALIFICATION
PLANNED / UNBLOCKED

WEB-TEST-CONTRACT-CLEANUP
PLANNED
```

## Configuration Manager CURRENT

El consumer final está publicado en `main@ee9a0401...`.

El composition root consume `ManagerModule` mediante:

```text
source_key
source_service
source_reader_service
source_history_service
projection_service
draft_validation_service
```

Los contracts legacy usados por el consumer anterior fueron removidos.

La composición publicada incluye un runtime local ejecutable para smoke/manual validation:

```text
LocalSourceStore
InProcessProjectionStore
Users
Navigation
Tools
KPI Configuration
KPI Definition
```

Ese runtime local no adjudica todavía el E2E productivo con Storage/Cosmos.

## Evidencia observada de este cierre

```text
git diff --check
PASS

legacy token scan scoped
0 matches

python compileall scoped
PASS

Configuration Manager local page boot
PASS / manual observation
```

No se atribuyen como PASS en este cierre:

```text
full pytest
full Ruff
full ADA regression
edit → validate → publish → project E2E
Storage/Cosmos Docker E2E
CI remoto
```

## Siguiente foco único

```text
ADA-CONFIGURATION-MANAGER-UI-CLEANUP
PLANNED / NEXT
```

Objetivo: corregir únicamente faltantes y problemas observables de UI del Manager ya recuperado, y revisar sólo los contratos que una anomalía concreta de esa UI demuestre como problemáticos.

No mezclar E2E local, Storage/Cosmos Docker, baseline Python ni otros frentes en ese incremento.
