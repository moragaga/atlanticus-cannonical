# ADA Command Center — Canonical Index

Estado: **CURRENT / ALARM SOURCE V3 + TOOL MANIFEST CLOSED / MANAGER UX IN PROGRESS**

Implementation checkpoint:

```text
moragaga/atlanticus@880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
```

| Archivo | Contenido | Estado |
|---|---|---|
| `01_PRODUCT_SCOPE.md` | Product scope. | CURRENT DIRECTION |
| `02_CURRENT_IMPLEMENTATION.md` | Qué existe hoy. | CURRENT / UPDATED |
| `03_WEB_APPLICATION.md` | Web/shell/identity. | CURRENT / PARTIAL |
| `04_CONFIGURATION_SCOPE.md` | Manager + Alarm Configuration v3. | CURRENT / UPDATED |
| `05_TOOL_TO_ALARM_CONFIGURATION.md` | Tool reconciliation -> Storage -> exact Alarm dependency manifest. | CURRENT / UPDATED |
| `06_ENGINE_AND_PROJECTIONS.md` | Base Projection, B.2, Effective/Live boundary. | CURRENT / UPDATED |
| `07_ANALYTICS_AND_STORYTELLING.md` | Analytics direction. | SEPARATE |
| `08_INITIAL_DASHBOARD.md` | Dashboard direction. | PROPOSED |
| `09_IDENTITY_NAVIGATION_PROFILES.md` | Identity/navigation. | CURRENT |
| `10_INITIAL_OUT_OF_SCOPE.md` | Initial non-goals. | CURRENT |
| `11_GOLDEN_PATH.md` | End-to-end path. | CURRENT / UPDATED |
| `12_SOURCE_LEDGER.md` | Source audit. | UPDATED |
| `13_OPEN_ITEMS.md` | Remaining gaps + foco de UX. | OPEN / MANAGER UX CURRENT PRIORITY |
| `14_TOOL_CATALOG.md` | Confirmed Tool Catalog + downstream manifest. | CURRENT / UPDATED |
| `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md` | Authored aggregate + durable snapshot v3. | CURRENT / UPDATED |
| `16_ALARM_LIVE_DELIVERY_CONTRACT.md` | Live Delivery contract. | PLANNED |
| `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md` | domain/tools + domain/alarms ownership. | CURRENT / UPDATED |
| `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md` | Acuerdos del editor y presentación diferida QUEUE_IN_QUEUE/CAROUSEL. | DECISION AGREED / DELIVERY PLANNED |

## CLOSED

```text
Alarm Domain extraction
Structured Alarm authoring
Command Center Tool Catalog V1
Pure B.2 resolver
Command Center Tools domain
ToolDependencyManifest
AlarmConfigurationSnapshot schema v3
validate -> publish Tool revision freeze
```

## Prioridad actual del Project

```text
Terminar y verificar UX del Alarm Configuration Manager
```

El adapter Cosmos y la composición de persistencia existen en main; su cualificación Azure end-to-end
permanece UNVERIFIED. Reconciliar por separado los documentos antiguos que aún declaran el adapter
como NEXT. No mezclar el incremento de UX con Materialization, Runtime Adoption o Live Delivery.
Ver `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md`.
