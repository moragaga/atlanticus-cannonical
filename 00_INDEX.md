# Atlanticus Canonical Context — Index

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@a31fce11d26a7c0a554d82de1813a4311522919b
```

Parent inmediato:

```text
90e89c376dfdfd182f0380b1d407127ecb7c9711
```

Canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical@a7adef2568d664ee31cb1b0eb1fe9f11ce2b9203
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
| `07_VALIDATION_BASELINE.md` | Evidencia de qualification y límites de validación. | CURRENT |
| `08_ROADMAP.md` | Orden de ejecución desde Baseline 1.0. | CURRENT |
| `09_OPEN_QUESTIONS.md` | Open items vigentes. | CURRENT |
| `10_MANAGER/` | Manager genérico, compositions y consumers administrativos. | CURRENT |
| `11_ADA_GENERIC/` | ADA Generic y cadena Tool → KPI → runtime. | CURRENT DIRECTION |
| `12_SOURCE_STORAGE/` | Source/Projection exact-release y storage. | CURRENT |
| `13_ADA_WEB/` | ADA Web y management. | CURRENT DIRECTION |
| `14_ADA_COMMAND_CENTER/` | Command Center y Alarm ownership. | CURRENT DIRECTION |
| `15_WEB_PLATFORM/` | Web platform, Users, Profiles, Access, Navigation y runtime. | CURRENT |
| `16_KPI_BACKEND_RECOVERY/` | Reprocessing/recovery KPI. | CURRENT DIRECTION |
| `17_DISTRIBUTION_AND_TOOLING/` | Generators, artifacts, scripts, docs y services. | CURRENT DIRECTION |
| `18_UNIVERSITY/` | Casos pedagógicos reales. | CURRENT DIRECTION |
| `ATLANTICUS_ENGINEERING_RULES.md` | Reglas de ingeniería. | CURRENT |
| `BASELINE_CLOSURE.md` | Qué queda congelado y qué no. | CURRENT |

## Estado de hitos relevantes

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-WEB-SURFACE
CLOSED / VERIFIED / CURRENT

PROFILES-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILE-OWNERSHIP-REALIGNMENT
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-CONTRACT
CLOSED / VERIFIED / CURRENT
```

Finding previo no cerrado:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

## Superficies administrativas

CURRENT reusable:

```text
Profiles Configuration Web surface
Profiles Manager composition
```

Pendientes separados:

```text
Users Administration UI
ADA Access Configuration UI
Manager final administrative composition
```

## Siguiente foco recomendado

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
PLANNED / NEXT / DESIGN FIRST
```

No diseñar desde cero lo que ya existe en código. Antes de implementar, verificar stores,
serialización de Projection y provenance/dependencies.
