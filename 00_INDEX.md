# Atlanticus Canonical Context — Index

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
```

Parent inmediato:

```text
856498c52f182cd531deae845c25bd51ae2ff4ea
```

Tree:

```text
3f27ad599c6dec610dff5317494a73b276d2ebc4
```

Canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical@deb493659b41c0d8fea5c70674002486b3b92cbc
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
| `10_MANAGER/` | Manager genérico, compositions y consumers administrativos. | CURRENT / UI REVIEW IN PROGRESS |
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

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

USERS-PROFILES-CONTRACT-REALIGNMENT
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

MANAGER-FINAL-ADMIN-COMPOSITION
CLOSED / VERIFIED / CURRENT

NAVIGATION-STANDALONE-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-CONFIGURATION-UI-PASS
CLOSED / VERIFIED MANUAL / CURRENT

MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Finding no cerrado:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

## Navigation CURRENT

Navigation Configuration no depende de Profiles core.

La integración con profiles usa un contrato neutral opcional:

```text
NavigationProfileOption
NavigationProfileOptionsProvider
```

ADA Configuration Manager adapta Profiles a ese contrato.

`allowed_profiles=()` significa público dentro de la autorización Navigation.

## Siguiente foco

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS / NEXT PAGE: HERRAMIENTA
```

Secuencia del mismo foco:

```text
visual desktop/page consistency
→ responsive/media-query audit
→ final test-contract cleanup/qualification
```

No abrir persistencia real, runtime authorization ni otros frentes backend durante este review.
