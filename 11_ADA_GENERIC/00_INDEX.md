# ADA Generic — Canonical Index

Estado: **CURRENT — SPECIALIZED APPLICATION EXTENSION CONTRACT + STATIC ALARM BASELINE CURRENT**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_SCOPE.md` | Frontera de ADA Generic. | CURRENT |
| `02_CURRENT_COMPOSITION.md` | Base runtime/composition, specialized extension, descriptor, Tool resolution y static baseline. | CURRENT |
| `03_CONFIGURATION_TO_RUNTIME.md` | Environment/persistence y Tool → binding → specialized application handoff. | CURRENT |
| `04_COLLECTOR_BOUNDARY.md` | Contrato Collector. | CLOSED / CURRENT |
| `05_FIRST_DELIVERABLE_VERTICAL.md` | Golden Path. | CURRENT / OPEN |
| `06_SOURCE_LEDGER.md` | Provenance y checkpoints. | AUDIT LEDGER |
| `07_TOOL_DELIVERY_ORDER.md` | Orden funcional/distribución. | CURRENT / SEPARATE TRACK |

## Estado

```text
ADA Generic                         0.2.26 / CURRENT
AdaApplicationDescriptor           CURRENT
AdaApplicationExtension            CURRENT
Tool structural resolution         CURRENT
OperationalRenderBinding           CURRENT
static Alarm Baseline              CURRENT / CLOSED
no-Tool degraded startup           CURRENT
Integrated Operations consumer     CURRENT
Tool hot reprojection refresh      PLANNED
dynamic alarm-live overlay         PLANNED / SEPARATE
distribution/artifact work         SEPARATE TRACK
```

## Regla

ADA Generic owns the reusable runtime/composition capabilities.

Specialized ADA products may:

```text
provide their own AdaApplicationDescriptor
append WebModules through AdaApplicationExtension
select their product page packages
consume the same OperationalRenderBinding resolved by Generic
```

They must not copy Generic bootstrap, Manager, identity, Navigation runtime, Tool resolution, KPI Collector or application lifecycle.

Do not move Alarm Engine business logic into Generic Web.

Do not recreate legacy Process layout roles.
