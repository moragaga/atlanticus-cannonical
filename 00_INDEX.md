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

## Autoridad de implementación en este cierre

Publicado en `moragaga/atlanticus:main` al iniciar el incremento:

```text
55cd6121e000a6af5d4f0dc0ea2e384f97a27f2a
```

Existe un working tree local posterior a ese checkpoint con el cutover de Users en progreso.

Ese working tree **no es todavía autoridad publicada** y no debe describirse como CURRENT hasta que se elimine toda compatibilidad legacy y se publique un nuevo checkpoint.

## Estado de hitos

```text
MANAGER-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT

NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-CONFIGURATION-LEGACY-CONTRACT-REMOVAL
IN PROGRESS / NOT ACCEPTED YET

PROJECTION-CORE-STALE-TEST-ALIGNMENT
CLOSED / VERIFIED

MANAGER-CONSUMER-GLOBAL-QUALIFICATION
VERIFIED GREEN ON CURRENT LOCAL WORKTREE
546 passed / 7 skipped
```

## Razón por la que Users todavía no está CLOSED

Durante el cutover local se introdujo compatibilidad permanente para schema v1:

```text
schema_v1.py
decode_users_profiles_schema_v1(...)
Source schema-v1 read branch
Projection schema-v1 read branch
```

Esa compatibilidad viola la regla vigente del incremento:

```text
LEGACY                          REMOVE
ADAPTERS / SHIMS / ALIASES     FORBIDDEN
DOBLE CONTRATO                  FORBIDDEN
OLD SCHEMAS IN RUNTIME CODE     REMOVE
```

Por tanto la suite GREEN no convierte el incremento en aceptable.

## Regla de ejecución refinada

Primero se completa la migración limpia y se elimina todo contrato/schema/adaptador anterior.

Después se ejecuta qualification scoped y global.

Los tests no justifican conservar comportamiento legacy.

## Siguiente foco único

```text
USERS-CLEAN-CUTOVER-COMPLETION
PLANNED / NEXT
```

Objetivo:

- eliminar toda lectura/decodificación schema v1 introducida para compatibilidad;
- eliminar tests dedicados exclusivamente a conservar schema v1;
- comprobar que no queda ruta legacy, adapter, shim, alias o doble contrato;
- sólo después ejecutar qualification completa;
- no abrir Tools/KPI hasta cerrar Users.

Atajos:

- Manager → `10_MANAGER/00_INDEX.md`
- Validation → `07_VALIDATION_BASELINE.md`
- Roadmap → `08_ROADMAP.md`
- Open items → `09_OPEN_QUESTIONS.md`
