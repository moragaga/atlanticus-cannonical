# ADA Command Center — Canonical Index

Estado: **CURRENT / ALARM MATERIALIZATION PROCESS v0.2.1 EXISTS / LOCAL OUTPUT CHANGE NEXT**

Corte: `atlanticus@b600ca591b56d0924aed752dfae6e9fab2c6f1d6`; canonical `772d15078c97802d58d8b658b0d5d5b928fa2ed5`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Este índice refleja sólo el delta de Alarm Materialization, no revalida frentes independientes.

| Archivo | Contenido | Estado respecto del hito |
|---|---|---|
| `01_PRODUCT_SCOPE.md` | Scope de producto. | CURRENT; sin cambios aquí. |
| `02_CURRENT_IMPLEMENTATION.md` | Inventario de código, incluido Materialization 0.2.1. | ACTUALIZADO. |
| `03_WEB_APPLICATION.md` | Shell/identity. | SIN CAMBIOS. |
| `04_CONFIGURATION_SCOPE.md` | Authoring Source v3. | CURRENT; sin cambios. |
| `05_TOOL_TO_ALARM_CONFIGURATION.md` | Reconciliation->Storage->manifest exacto. | CURRENT; sin cambios. |
| `06_ENGINE_AND_PROJECTIONS.md` | Cosmos operacional como entrada, artefactos locales como salida objetivo. | ACTUALIZADO. |
| `07_ANALYTICS_AND_STORYTELLING.md` | Analytics independiente. | SEPARATE. |
| `08_INITIAL_DASHBOARD.md` | Dashboard conceptual. | SEPARATE. |
| `09_IDENTITY_NAVIGATION_PROFILES.md` | Identity y navegación. | SIN CAMBIOS. |
| `10_INITIAL_OUT_OF_SCOPE.md` | No-objetivos. | SIN CAMBIOS. |
| `11_GOLDEN_PATH.md` | Flujo de extremo a extremo y estados reales/propuestos. | ACTUALIZADO. |
| `12_SOURCE_LEDGER.md` | Genealogía anterior del Manager. | HISTORICAL; evidencia actual nueva en `04_ALARM_ENGINE/11_SOURCE_LEDGER.md`. |
| `13_OPEN_ITEMS.md` | Frentes de Manager UX y otros temas. | AÚN ABIERTO EN SU PROPIO FOCO; no resolver aquí. |
| `14_TOOL_CATALOG.md` | Confirmed Tool Catalog. | CURRENT; sin cambios. |
| `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md` | Aggregate y snapshot v3. | CURRENT; sin cambios. |
| `16_ALARM_LIVE_DELIVERY_CONTRACT.md` | Live Delivery conceptual posterior, por EFFECTIVE exacto. | PLANNED; no tocar en Materialization. |
| `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md` | Owners Domain. | CURRENT; sin cambios. |
| `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md` | UX y presentación diferida. | SEPARATE; conflicto de sincronización sigue OPEN. |

Foco único tras este cierre: **reemplazar salida Cosmos del proceso Materialization por salida local coherente en volumen, y validarla sin infraestructura Cosmos real**. La existencia de proceso no demuestra aún E2E; sus consumidores se conectarán después.
