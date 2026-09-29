# ADA Command Center — Canonical Index

Estado: **CURRENT — cortes independientes B2c.7 (Engine/Delivery, 2026-09-28) y B1d (Tool Catalog Web, 2026-09-29)**. La evidencia Engine sigue referida a `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`; B1d está incorporado en `a518ff98c6303220e24ae3c645d3982e657fd22e` y presente en `main@caced5d7711cf059d36ec61aecc9b3e9629bd41f`. Decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`; base de lectura de estos documentos `ec16bd2ccf0ae06065b8ee1d3a231ef4d2cbac57` (HEAD posterior `a5bb42157ee7a5dd2fd64ccc43fa4519628ce25c`, sin cambios en rutas tocadas). No se recalifican Analytics ni otros dominios.

Este índice conserva los gates históricos de Engine y añade **únicamente** la implementación/qualification B1d del catálogo y el diseño pendiente de su extracción Web/Starter. Los estados por archivo se delimitan al contenido efectivamente revisado.

| Archivo | Contenido | Estado en este hito |
|---|---|---|
| `01_PRODUCT_SCOPE.md` | Scope de producto. | SIN CAMBIOS; no recalificado. |
| `02_CURRENT_IMPLEMENTATION.md` | Inventario B2c.7 + estado real B1d, con límites de qualification. | ACTUALIZADO B1d. |
| `03_WEB_APPLICATION.md` | Host Manager actual y Starter distribuible pendiente. | ACTUALIZADO B1d. |
| `04_CONFIGURATION_SCOPE.md` | Source v3/authoring y límites de aceptación B1d. | ACTUALIZADO, sin cambio contractual. |
| `05_TOOL_TO_ALARM_CONFIGURATION.md` | Reconciliation→Storage→manifest exacto y prueba física local B1d. | ACTUALIZADO. |
| `06_ENGINE_AND_PROJECTIONS.md` | Materialization local, EFFECTIVE, outputs Engine, recepción Delivery y proyecciones futuras. | REEMPLAZO B2c.7 ADJUNTO. |
| `07_ANALYTICS_AND_STORYTELLING.md` | Analytics independiente. | SEPARATE; no afirmar implementado por FACTS. |
| `08_INITIAL_DASHBOARD.md` | Dashboard conceptual. | SEPARATE. |
| `09_IDENTITY_NAVIGATION_PROFILES.md` | Identity y navegación. | SIN CAMBIOS. |
| `10_INITIAL_OUT_OF_SCOPE.md` | No-objetivos. | SIN CAMBIOS. |
| `11_GOLDEN_PATH.md` | Flujo E2E con evidencia B1d delimitada y B2c.7 conservado. | ACTUALIZADO. |
| `12_SOURCE_LEDGER.md` | Genealogía Manager más corte B1d; Engine en `04_ALARM_ENGINE/11_SOURCE_LEDGER.md`. | ACTUALIZADO. |
| `13_OPEN_ITEMS.md` | Extracción Tool Web, Starter, limpieza y UX Alarm separada. | ACTUALIZADO. |
| `14_TOOL_CATALOG.md` | Catálogo, Manager B1d, extracción Web decidida/pendiente. | ACTUALIZADO. |
| `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md` | Aggregate/Source v3; evidencia local y modal pendientes. | ACTUALIZADO, contrato v3 intacto. |
| `16_ALARM_LIVE_DELIVERY_CONTRACT.md` | Contrato Live conceptual con estado actualizado del productor y receptor. | REEMPLAZO B2c.7 ADJUNTO; Live aún PLANNED. |
| `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md` | Owners Domain/Web y extracción sin dependencia de host. | ACTUALIZADO. |
| `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md` | UX/routing y defectos de modal/desactivación diferidos. | ACTUALIZADO; conflictos OPEN. |

**Focos independientes:** para B1d Web, extraer Tool Catalog a biblioteca reutilizable y luego componer/distribuir Starter; sólo entonces cerrar limpieza y actualizar estado de implementación. Para Engine, conserva por separado qualification/distribución Docker. `AlarmLiveProjection`, Capture e History/Analytics permanecen SEPARATE. No actualizar estados de otros proyectos por inferencia.
