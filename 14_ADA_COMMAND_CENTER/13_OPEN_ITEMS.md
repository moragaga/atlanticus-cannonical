# ADA Command Center — Open Items

Estado: **CURRENT / desarrollo modular activo; Alarm Engine B2c.5d CLOSED local e integrado; composición B2c.6 PLANNED; Web deactivation hasta fin del turno OPEN**.

Corte de este reemplazo: `moragaga/atlanticus@a799dc15105d3e037f36ab77129ef0cfa8999013`, decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`, canonical base `46877f174513b2475f17b7dc739cd43951fa4ed0`. Este documento actualiza **sólo** los hechos de Alarm Engine y la nueva observación sobre la autoría Web; no revalida por implicación las demás áreas de Command Center.

## CLOSED / CURRENT con evidencia delimitada

- Extracción del dominio Alarm; Tool Catalog v1; autoría estructurada por Rules/Messages; ToolDependencyManifest y captura de historial Tool; `AlarmConfigurationSnapshot` schema v3 con manifest Cn exacto; protección de drift en Validate/Publish; identidad/revisión Tool correlacionadas con release Alarm.
- `resolve_alarm_configuration` B.2 puro; publicación Materialization READY/BLOCKED local; lector exacto; B1 reference/plan; B2a WAL V1/V2; B2b Effective Head exacto; ejecución y sesiones B2c.
- B2c.5c (lectura por registro actual y requisitos por evaluador), regresión local reportada **436 PASS**; B2c.5d (catálogo y ejemplo separado), **7 específicas + 443 regresión PASS, Ruff PASS y 40 archivos formateados**. Presencia de los archivos del ejemplo y registro vacío comprobada en a799dc1.
- Las suites de componentes Web anteriores y registros de composición Cosmos/local no acreditan por sí mismos despliegue Azure, pruebas en navegador host real ni conformidad final de UI.

## OPEN — nueva observación: desactivación hasta fin del turno

**VERIFIED en main:** en la autoría Web de alarmas `default_deactivation.max_duration_hours` se representa con `_number_field`. El contrato `AlarmDeactivationDefinition` y el override de mensajes usan `max_duration_hours: int|None`; para desactivación habilitada exigen entero **1..12**. El Engine dispone de `effective_until` UTC para solicitudes/efectos, pero esto **no** prueba que un operador o configurador pueda elegir la expresión **'hasta el fin del turno'** como máximo de desactivación.

**OPEN / requisito observado por el usuario, no solucionado:** la Web actual ofrece un máximo numérico y no permite establecer fin del turno como máximo. Antes de modificar UI se requiere contrastar reglas de negocio (qué significa máximo; instante de inicio, zona/calendario operacional Mine/Plant, límites 1..12, aprobación y overrides Message), capa de configuración/publicación, contratos del Core y calendario existente. **No** añadir enum, adaptador legacy, campo adicional o conversión silenciosa sin decisión. Este frente debe permanecer **SEPARATE de B2c.6**.

## Próximo foco del frente de este cierre: B2c.6

Auditar la **composición ejecutable real** ya existente de Alarm Runtime: `build_alarm_runtime_process(...)` exige inyectar `evaluator_registry` y `source_loader`; `build_alarm_source_adapter(...)` está disponible. `catalog/registry.py` permanece vacío deliberadamente, `catalog/examples/threshold` es sólo referencia. Identificar entrypoints y conexiones reales antes de implementar wiring mínimo, sin datasets reales y sin tocar Core/Domain/Web.

## Otros OPEN que no se declaran resueltos

- **Web authoring UX/host/browser:** validar la experiencia real, ayudas, gestión de familia, feedback, persistencia y flujos Source/Projection que no estén acreditados por suites unitarias. Ver `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md`.
- **UI Tool revision:** notificación explícita de que Cn cambió respecto del pin guardado, si aún falta aceptación de producto.
- **Routing visual:** conceptual visual targets independientes frente a sincronización actual del editor, decisión específica pendiente.
- **Proyección Cosmos:** adapter/composición local y Cosmos presentes en código; despliegue Azure E2E UNVERIFIED.
- **Productores Tool GREEN/evaluator qualification:** UNVERIFIED operacionalmente.
- **Runtime/Delivery/findings y Live:** no equiparar artefactos Materialization locales con Live Delivery Web ya implementado; flujo externo exacto pendiente.
- **Management Capture/Projection, History y Analytics:** PLANNED / SEPARATE, no acceso directo Web al WAL.
- **Source v2 histórico en deployments:** no hay decoder legacy actual; inventario/migración sólo si se demuestra necesidad de despliegue.
- **Python:** objetivo transversal 3.14.7 vs metadata Command Center `==3.14.2`; frente de distribución separado.
- **OperationalScope semana para PI/KPI Runtime:** ausente; `ShiftScope.CURRENT_WEEK` y FABRICA_PLANES WEEKLY no sustituyen ese contrato. Registrar como OPEN en KPI, no implementar aquí.

**No reabrir workstreams durante el cierre.** Las decisiones de semántica fin de turno y forma final de presentación Web corresponden a otro foco, posterior y explícitamente autorizado.
