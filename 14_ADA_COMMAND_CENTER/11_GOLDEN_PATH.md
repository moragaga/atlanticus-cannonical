# ADA Command Center — Golden Path

Estado: **PARTIALLY IMPLEMENTED: C1 Tool services Web CLOSED estructuralmente; Materialization/Engine/Delivery input B2c.7 validados históricamente en local; durable Alarm E2E, Live/Web/History y Docker distribuido UNVERIFIED/PLANNED**. Corte C1 2026-09-29: `atlanticus:main@3961385aecd0eb7e373018fc25e509a71dccc409`. Corte B2c.7 histórico: `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`. No convertir el nuevo commit en una nueva qualification de Engine.

## Recorrido con estados delimitados

| Paso | Owner | Estado según evidencia de los distintos cortes |
|---|---|---|
| Tool owners publican Projections; Manager reconcilia Cn | Tool/Manager Web | CURRENT preexistente; qualification física local B1d histórica. |
| Confirmed Tool Catalog Cn -> Blob CURRENT | `web/tools/catalog` | CURRENT; ownership de `backend/tools` SUPERSEDED en C1. Azure físico UNVERIFIED. |
| Discovery/confirm por conexiones Cosmos nombradas | `web/tools/discovery-cosmos` | CURRENT C1; sin catálogo Cosmos de salida. |
| UI Tool Catalog capability reutilizable | `web/tools/catalog-manager` | CURRENT; host temporal compone la biblioteca, sin duplicación UI. |
| Alarm authoring pin Cn, Validate/Publish y drift guard | Alarm Web | CURRENT preexistente. |
| Source release Rn congela ToolDependencyManifest(Cn) schema v3 | Alarm Configuration | CURRENT. |
| Persistencia/proyección operativa de entrada Alarm | Web/Materialization | CURRENT en código; infraestructura Cosmos física E2E UNVERIFIED. |
| B.2 adquiere proyección, aplica qualification y resuelve | Materialization | CURRENT; JSON manual actual, productor GREEN real UNVERIFIED; automatización C3 PLANNED. |
| READY local manifest+Runtime/Delivery o BLOCKED | Materialization | CURRENT; salida Cosmos B.2 monolítica antigua SUPERSEDED. |
| B1 exact pin; WAL adoption V1/V2; EFFECTIVE derivado | Engine/Persistence | CURRENT; recovery local validado en cortes previos. |
| Runtime ejecuta sesión efectiva y publica CURRENT completo v1 | Engine | CURRENT; tests locales B2c.7. |
| Runtime exporta sólo commits durables como FACTS v2 encadenados | Engine | CURRENT; tests locales B2c.7. |
| Job separado recibe CURRENT/FACTS con pin/cursor propio | Delivery input | CURRENT en código; comportamiento de replay FACTS objetivo SUPERSEDED para futuro C4. |
| Engine→Delivery volumen controlado y re-instanciación | Gate local | CLOSED local histórico, no Docker/multi-host. |
| Tool services Web C1 test/wheels/importaciones host | Gate local | CLOSED estructural: suites Web, Backend conjuntas y Ruff; **instalación aislada y aceptación visual UNVERIFIED**. |
| Tres jobs con APPLICATION/rutas/Source Key unificadas y Cosmos físico por contrato | C2 | PLANNED; `.env.detail` actuales NO cumplen todavía. |
| Qualification verificable automática | C3 | PLANNED / BLOCKED hasta definir verificadores reales y productor. |
| Delivery sólo último CURRENT, sin backlog FACTS | C4 | PLANNED; NO modificar exportación FACTS Runtime. |
| Contrato evidencia técnica interno y `.env.detail` depurada | C5 | PLANNED; claves de evidencia técnica final OPEN. |
| Starter Command Center distribuible, full Web productiva | Web | PLANNED. El host actual es temporal. |
| Live cause/visibility/priority, AlarmLiveProjection | Live Delivery | PROJECT CONTRACT AGREED / NOT IMPLEMENTED. |
| Management Capture, History/Analytics y dashboard | Servicios separados | PLANNED / SEPARATE. |

## Qualification B1d — alcance histórico, no recalificado por C1

El usuario observó `prepare --apply`/`prepare` sobre dos bases Cosmos de qualification con contenedor Tool Projection, Source→Projection de Process/Integrated Operations, discovery READY en dos conexiones y confirmación manual seguida de verificación del Blob `conciencia_situacional/command-center/tool-catalog/current.json`. Es prueba local/Azurite/Cosmos Emulator del corte B1d; no es preloading ni despliegue productivo. `verify-alarm` durable no encontró Source/Projection Alarm en aquel corte. Esa ausencia no reabre la qualification del catálogo ni permite declarar completo el Golden Path.

## C1 — evidencia nueva acotada

Git remoto `3961385a...` confirma `web/tools/catalog`, `web/tools/discovery-cosmos` y `web/tools/catalog-manager`; `backend/tools` ya no tiene archivos versionados. El usuario regeneró locks, ejecutó pruebas, Ruff, mirrors y wheel builds de ambas bibliotecas reubicadas; smoke de importaciones del host pasó. Esto cierra **la migración de ownership**, no Browser UI, instalación aislada del wheel, autenticación productiva ni Starter.

## Separaciones obligatorias — congeladas

```text
Rn/Cn y manifest Tool son exactos al publicar Source release v3.
VALID_AT_SAVE != READY != EFFECTIVE.
READY publica Runtime y Delivery juntos, con un mismo AlarmResolutionKey/pin exacto.
BLOCKED conserva diagnóstico, sin artefactos ejecutables.
INVALID != REMOVED; DISABLED != REMOVED; TRACE_ONLY != REMOVED.
WAL -> DURABLE -> MATERIALIZED -> EFFECTIVE projection.
Engine CURRENT v1 completo (snapshot) != FACTS v2 encadenados (hechos durables).
Engine export cursor != Delivery consumption cursor (CURRENT actual en código).
Delivery input receiver != AlarmLiveProjection.
C1 Web Tool Catalog/Discovery/UI no introduce ownership Backend para ellos.
```

Cn posterior no cambia retroactivamente Rn/Cn. Runtime y Delivery no reobtienen su configuración desde Cosmos: utilizan versión local exacta acorde a EFFECTIVE. Nuevo requisito de C4: cuando esté implementado, Delivery leerá sólo el último CURRENT disponible y no consumirá historial atrasado; durante C1 la implementación actual CURRENT+FACTS sigue vigente. Runtime FACTS durables subsisten para funciones independientes.

## Fronteras abiertas / siguiente foco actual

**Foco único siguiente C2:** auditar/comparar decisiones y código de identidad/rutas operacionales de los tres jobs, Source Key compartida y `ALARM_CONFIGURATION_PROJECTION_STORAGE_RESOURCE`; mantener contenedor Blob en env y retirar contenedor Cosmos ambiental sólo junto al cambio de composición. No incluir C3 productor Qualification, C4 Delivery, C5 evidencia técnica ni Starter. Qualification Engine/Delivery Docker separada continúa abierta en su otro frente; el «próximo entregable Docker» del corte B2c.7 permanece registro histórico y NO desplaza el foco C2 de este traspaso.
