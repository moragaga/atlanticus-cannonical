# Atlanticus Canonical Context — Index

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@6dd09a6f24370bbad8ae358b6d5d7c6ea9aeba4a
```

Parent inmediato:

```text
4e008055ddc551e6c08a7d87715340c8c7cd149e
```

Canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical@61da5829c6a1f8ec936d46e5a7ec02965b5e4743
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
| `10_MANAGER/` | Manager genérico, Configuration Manager y consumers administrativos. | CURRENT |
| `11_ADA_GENERIC/` | ADA Generic, ownership ADA y cadena Tool → KPI → runtime. | CURRENT DIRECTION |
| `12_SOURCE_STORAGE/` | Source/Projection exact-release y storage. | CURRENT |
| `13_ADA_WEB/` | ADA Web y management. | CURRENT DIRECTION |
| `14_ADA_COMMAND_CENTER/` | Command Center y Alarm ownership. | CURRENT DIRECTION |
| `15_WEB_PLATFORM/` | Web platform, Users global registry, Activity, startup y projections. | CURRENT |
| `16_KPI_BACKEND_RECOVERY/` | Reprocessing/recovery KPI. | CURRENT DIRECTION |
| `17_DISTRIBUTION_AND_TOOLING/` | Generators, artifacts, scripts, docs y services. | CURRENT DIRECTION |
| `18_UNIVERSITY/` | Casos pedagógicos reales. | CURRENT DIRECTION |
| `ATLANTICUS_ENGINEERING_RULES.md` | Reglas de ingeniería. | CURRENT |
| `BASELINE_CLOSURE.md` | Qué queda congelado y qué no. | CURRENT |

## Estado de hitos relevantes

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

KPI-DEFINITION-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS

USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT

USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
PLANNED

ACCESS-PROFILES-CONFIGURATION
PLANNED

WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

## Users CURRENT

Users ya no es una configuration Source.

```text
web/capabilities/users/
├── activity
├── blob
├── core
└── cosmos
```

Removido del árbol CURRENT:

```text
web/capabilities/users/configuration
web/capabilities/users/projection-cosmos
web/compositions/users-manager
```

El registro durable de Users se representa mediante `UsersRegistryStore` y el
provider Blob CURRENT usa por defecto:

```text
users/users.json.gz
```

El store promovido/runtime CURRENT usa Cosmos con:

```text
document_type = atlanticus_user
schema_version = 1
```

ADA Configuration Manager ya no registra Users como `ManagerModule` ni consume
Users Source/Projection.

## Evidencia observada del cierre

```text
Python 3.14.7
VERIFIED in local qualification

uv lock
PASS

uv sync
PASS

web pytest
416 PASS / 7 SKIPPED

Ruff scoped: Identity + Users affected packages
PASS

ADA Configuration Manager final scoped qualification
PASS / user-observed
```

No se atribuyen como PASS:

```text
full Ruff workspace after final commit
full ADA regression outside the consumer package
CI remoto
production persisted-data migration
concrete Entra/Graph directory provider
```

## Siguiente foco único

```text
USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```

Objetivo: inspeccionar y cortar únicamente el estado persistido real de Users al
contrato CURRENT, sin adapters runtime y sin destruir información de Profiles que
todavía deba preservarse para su lifecycle independiente.
