# ADA Web — Canonical Index

Estado: **CURRENT — INTEGRATED OPERATIONS INITIAL UI CURRENT / KPI CONTENT PLANNED**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_CURRENT_BASELINE.md` | Baseline Web funcional actual. | CURRENT |
| `02_TESTING_POLICY.md` | Política de pruebas. | CURRENT |
| `03_SHELL_BOUNDARIES.md` | Shell boundaries. | CURRENT/HISTORICAL |
| `04_QUALIFICATION_HISTORY.md` | Evidencia histórica. | HISTORICAL |
| `05_SOURCE_LEDGER.md` | Provenance del baseline Web. | AUDIT LEDGER |
| `06_INFRASTRUCTURE_STARTUP.md` | Runtime Docker, resource preparation y emuladores. | CURRENT |
| `07_ALARM_MANAGEMENT_FRONTEND.md` | Alarm management UI. | OTHER FOCUS |
| `08_INTEGRATED_OPERATIONS_PRESENTATION.md` | Specialized application, complete initial Dashboard layout, responsive focus and static baseline coordination. | CURRENT |

## CURRENT Web structure relevant to Integrated Operations

```text
Tool Projection READY
→ ToolStructure
→ OperationalRenderBinding
→ same binding to:
     Generic static Alarm Baseline
     Integrated Operations DashboardContext
→ one Dashboard page /
→ internal Mine + Plant surfaces
→ product-local 9-component / 22-card static presentation
```

## Integrated Operations initial UI status

```text
specialized descriptor                    CURRENT
additive Generic extension                CURRENT
Dashboard application module              CURRENT
Mine/Plant internal composition           CURRENT
9 operational visual components           CURRENT
22 visual cards                           CURRENT
responsive presentation                   CURRENT
Mine/Plant focus                          CURRENT
static baseline focus coordination        CURRENT
Tool identity mapping in bindings.py      PLANNED / OPEN
KPI-driven card content                   PLANNED
```

## Responsive modes CURRENT

```text
<1280       mobile
            Mine + Plant vertical
            no Mine/Plant controls
            alarm management/status/baseline hidden by Integrated Operations

1280-1365   tablet
            one scope at a time
            only control toward opposite scope
            focused static baseline points redistributed over full width

1366-2559   desktop
            9-track overview
            Mine focus or Plant focus
            close returns to overview
            focused static baseline follows visible scope

>=2560      videowall
            fixed overview
            no presentation controls
```

The `350`, `480`, `1536` and `1920` breakpoints remain presentation refinements inside these broader modes.

## Static Alarm Baseline status

```text
projection/domain ownership                CURRENT / GENERIC
static baseline qualification              VERIFIED
Integrated Operations browser path         VERIFIED
focus-aware point filtering/repositioning  CURRENT product presentation
dynamic alarm overlay                      PLANNED / SEPARATE
```

Integrated Operations does not own Alarm Engine business rules.

## Reuse boundary OPEN

The current Dashboard card shell and binding declarations are product-local under:

```text
ada.web.application.integrated_operations.modules.dashboard
```

Possible overlap with existing reusable UI under `scopes/ada/web/ui` was identified at closure but was not evaluated.

Status:

```text
OPEN / next design boundary
```

Do not migrate or duplicate the card shell before comparing the existing reusable ADA UI capability and freezing the identity/render contract.

## Reference sources

The following remain references only:

```text
moragaga/isolated-web-functions:main
moragaga/__temporal_ada_latest:main
moragaga/atlanticus-multi-stage:main
```

They informed visual/layout behavior but do not represent Atlanticus contracts or ownership.
