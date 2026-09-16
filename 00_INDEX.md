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
| `11_ADA_GENERIC/` | ADA Generic y orden de Tools. | CURRENT DIRECTION |
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
moragaga/atlanticus@a065f45c55a527c96ce333705465487e95f0a737
```

Ese checkpoint contiene el cierre del clean cutover de Users.

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

TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED / NEXT

KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED

KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED
```

## Users clean cutover

Removido del runtime CURRENT:

```text
schema_v1.py
decode_users_profiles_schema_v1(...)
Source schema-v1 read branch
Projection schema-v1 read branch
tests dedicados a preservar lectura schema v1
```

Regla vigente:

```text
LEGACY                          REMOVE
ADAPTERS / SHIMS / ALIASES     FORBIDDEN
DOBLE CONTRATO                  FORBIDDEN
OLD SCHEMAS IN RUNTIME CODE     FORBIDDEN
```

## Qualification de cierre

```text
ruff scoped
PASS

pytest scoped
99 passed

forbidden scan sobre código CURRENT
PASS / zero matches

full Web pytest
545 passed
7 skipped
0 failed

git diff --check HEAD^..HEAD
PASS

git status --short
CLEAN
```

## Siguiente foco único

```text
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER
PLANNED / NEXT
```

No asumir que Tools requiere los mismos cambios que Users.

Atajos:

- Manager → `10_MANAGER/00_INDEX.md`
- Validation → `07_VALIDATION_BASELINE.md`
- Roadmap → `08_ROADMAP.md`
- Open items → `09_OPEN_QUESTIONS.md`
