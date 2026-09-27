# Alarm Engine — Decision Index

Estado: **CURRENT / LOCAL MATERIALIZATION OUTPUT DECIDED — IMPLEMENTATION PENDING (2026-09-27)**

| ID | Tema | Estado |
|---|---|---|
| ALARM-DEF-B1 | Contrato B.1 de Alarm Definition | HISTORICAL FROZEN, con refinamientos posteriores identificados. |
| ALARM-PROJ-B2 | Proyección/publicación B.2 | HISTORICAL REFINED; SharePoint/Tool latest y salida de materiales ya no describen automáticamente la arquitectura actual. |
| ALARM-B2-PURE-RESOLVER | Resolver puro determinista | CURRENT / IMPLEMENTED. |
| ALARM-TOOL-MANIFEST | ToolDependencyManifest exacto en snapshot publicado | CURRENT / IMPLEMENTED. |
| ALARM-SOURCE-V3 | `AlarmConfigurationSnapshot` schema 3 | CURRENT / IMPLEMENTED; v2 SUPERSEDED sin legacy. |
| ALARM-TOOLS-FREEZE | Pin Cn y rechazo de drift Validate/Publish | CURRENT / IMPLEMENTED. |
| ALARM-CONFIG-PROJECTION | Builder, codec y stores Local/Cosmos | CURRENT / IMPLEMENTED; E2E real UNVERIFIED. |
| ALARM-ROUTING-STRICT | Sólo siguiente nivel Process->Integrated->Strategic | CURRENT / FROZEN / IMPLEMENTED. |
| ALARM-MATERIALIZATION-EXECUTABLE | Job 0.2.1 adquiere proyección y ejecuta B.2 | CURRENT EN CÓDIGO; validación integrada pendiente. |
| ALARM-MATERIALIZATION-COSMOS-OUTPUT | Resultado monolítico escrito en Cosmos | CURRENT EN CÓDIGO / SUPERSEDED EN DISEÑO; debe reemplazarse limpiamente. |
| ALARM-MATERIALIZATION-LOCAL-OUTPUT | Artefactos Runtime y Delivery en volumen local coherentes/versionados | DECIDED / PLANNED; layout físico y tests por definir sobre código existente. |
| ALARM-QUALIFICATION-PRODUCERS | Tool GREEN y Evaluator operacionales reales | OPEN / UNVERIFIED; no duplicar productores. |
| ALARM-RUNTIME-CONSUMPTION | Lectura local por versión exacta, adopción controlada | DECIDED COMO FRONTERA / FUTURE INCREMENT. |
| ALARM-VISUAL-ROUTING-OWNERSHIP | Independencia conceptual vs sincronización actual del editor | CONFLICT / NO TOCAR AQUÍ. |

## Refinamientos de decisiones anteriores

1. SharePoint como autoridad universal y re-resolución Tool contra latest pertenecen a documentación histórica donde dominios ya migraron: Source durable objetivo Blob; Rn conserva el manifest exacto Cn. No reactivar lookups de Tool.
2. La alternativa de publicar artefactos B.2 en Cosmos (implementada en el proceso 0.2.1) está **SUPERSEDED como objetivo**, por la decisión explícita de que Materialization publique en volumen local. El código no cambia hasta el siguiente incremento.
3. La idea de que Engine/Delivery descarguen la materialización desde Cosmos queda **SUPERSEDED**. Deben cargar localmente la revisión adoptada; esto no decide hoy cómo publicar la futura proyección Live a Web.
4. La posible equiparación `READY == EFFECTIVE` no está autorizada. Sólo Runtime Adoption cambia configuración efectiva.
5. El diseño del directorio local con nombres `.json` fue una **propuesta no congelada**; no transformarlo en esquema implementado sin auditar herramientas existentes y acordar el contrato.
6. El routing histórico con saltos o rutas same-tier fue reemplazado por la policy strict ya implementada.

## Fuentes

`atlanticus@b600ca591b56d0924aed752dfae6e9fab2c6f1d6`; canonical inspeccionado `772d15078c97802d58d8b658b0d5d5b928fa2ed5`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Comparar documentos históricos con la realidad actual y registrar conflictos; no mutar repositorios durante este cierre.
