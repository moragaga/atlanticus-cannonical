# ADA Command Center — Alarm Configuration Authoring Model

Estado: **CURRENT — Snapshot v3, editor guiado y routing estricto en código; deactivation fin de turno OPEN / no definido**. Corte de este frente 2026-09-28: `atlanticus@a799dc15105d3e037f36ab77129ef0cfa8999013`. Los gates anteriores de Web eran de componente, **no** aceptación host/browser global.

## Agregado editable y versión durable

```text
AlarmConfiguration
  rules
  messages

AlarmConfigurationSnapshot
  configuration: AlarmConfiguration
  tool_dependencies: ToolDependencyManifest
  schema_version: 3
```

La familia se deriva de `AlarmIdentity.family_key` y de mensajes con `scope=FAMILY`; no hay entidad Family persistida separada. `alarm_key` es identidad estable; `rule_name`, display y título no la sustituyen. `confirmed_tool_catalog_revision` se obtiene del manifest exacto congelado con la release Rn. Source v2 está SUPERSEDED y no tiene decoder compat actual. Conservar referencias de Rules activas/inactivas, origen Tool, steps habilitados/deshabilitados, targets visuales y estructuras Tool exactas para reconstrucción sin latest Tool Catalog.

## Workspace / Save / Validate / Publish — CURRENT

El editor opera sobre `AlarmConfiguration` y metadata sidecar de workspace `_confirmed_tool_catalog_revision`, que no es parte del agregado. Save Draft pinnea Cn; Validate comprueba configuración intrínseca, pin actual y referencias Tool definidas; Publish repite y congela subset exacto en Source v3. Si Cn cambia, requiere acción deliberada de actualización. Releases históricas Rold/Cold no se reescriben. `VALID_AT_SAVE != B.2 READY != EFFECTIVE`. Un borrador transitoriamente incompleto no debe publicarse silenciosamente.

El editor guiado existente organiza familias, Rules/Messages y la selección Tool/Component/Subcomponent y routing; hay suites locales previas del componente. La aceptación visual/responsive y host/browser completo no queda certificada por las pruebas del Engine.

## Routing FROZEN; presentation boundary OPEN

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END
```

Sin saltos, mismo nivel o retrocesos. C1 y C2 pueden no declarar destinos; C3 solo origen; C1 inmediato y C2 tiempos positivos/offsets calculados en B.2. Web comparte `next_routing_tool_kind` con Domain. Configuración histórica inválida debe permanecer visible para corrección, sin eliminación automática ni cambio implícito de criticidad. `routing_tools` puede incluir Strategic, `tools` de visualización contiene Process/Integrated Operations; no añadir target visual Strategic sin contrato expreso.

Cada Rule conserva `visual_targets` con Tool, `component_keys`, subcomponentes identificados por `(owner_component_key,subcomponent_key)` y `process_projection_mode` exclusivo de Process. `QUEUE_IN_QUEUE` para Integrated Operations y `CAROUSEL` para Process corresponden a estrategia futura de visualización, **no campos Source v3 ni scheduler nuevo**. Sigue **OPEN / CONFLICT** la independencia conceptual de targets visuales vs sincronización actual desde routing del editor. Fuente contextual: `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md`.

## Parametrización de evaluación — precisión B2c.5d

Las Rules conservan `evaluator_key` y `parameters: Mapping[str,str|float|bool]` (sin `None`, listas, diccionarios anidados ni expresiones). Los parámetros **son opcionales** como inputs de negocio: la Web no debe crear una obligación general de límite/factor para todas las alarmas ni usarlos para resolver automáticamente columnas, particiones o fuentes del evaluator. El desarrollador decide los parámetros que lee y cualquier validación propia. Los requisitos de datos para lógicas nuevas del catálogo se definen manualmente en el contrato del evaluador y no son un catálogo de parámetros global. El ejemplo controlado B2c.5d usa `limit` opcional con default demostrativo 80.0; **no** es una Rule/producto registrado en producción.

El par `(family_key,evaluator_key)` resuelve implementación de código; `alarm_key` identifica la Rule configurada específica. `EvidenceSnapshot` flexible es salida de evaluación en backend; la Web de configuración no decide el lifecycle, WAL ni almacenamiento de evidencia.

## OPEN / nueva observación: límite de desactivación hasta fin del turno

**VERIFIED / CURRENT:** en `web/alarms/configuration/web/layout.py`, `default_deactivation.max_duration_hours` se muestra con `_number_field`. Domain define `AlarmDeactivationDefinition` y `MessageDeactivationDefinition` con `max_duration_hours: int|None`; si deactivation está habilitada se exige entero **1..12**. Este es el contrato de authoring y publicación realmente soportado a la fecha. No hay selector contractual confirmado para un máximo definido como **fin del turno**.

**OPEN / necesidad registrada por el usuario:** poder fijar como máximo de desactivación el término del turno operativo en lugar de una cantidad numérica fija. Esto puede atravesar Web/Domain/Materialization/Core/calendario: antes de diseñar UI decidir semántica exacta, Mine/Plant y turno aplicable, zona/instante, aprobación, overrides de mensajes y compatibilidad con la restricción actual 1..12. No introducir ningún campo, enum, algoritmo o migración en este cierre; tampoco interpretar la existencia del `effective_until` UTC de Core como cumplimiento de la autoría pedida.

## Delta de aceptación Web B1d (sin cambiar Source v3)

La cualificación B1d verificó el catálogo consolidado y su presencia en el Manager, además de una Alarm Source bajo proveedor `local`; **no** acreditó Source Blob/Projection Cosmos durable de alarmas. La publicación y la proyección son acciones separadas. La experiencia de creación observada no equivale a aceptación integral del flujo durable.

**OPEN / siguiente foco específico Web, tras extraer Tool Catalog y normalizar el Starter:** definir el tiempo de desactivación/«fin del turno» sin asumir un contrato nuevo, y cerrar el modal de configuración **sólo cuando guardar termine con éxito**, preservando mensajes de validación y errores si falla. No inferir que estas dos correcciones estén implementadas por el hecho de haber usado la UI.

## Frontera siguiente de este cierre

Este asunto Web es **separado** del próximo incremento técnico B2c.6, que auditará la composición ejecutable real de Alarm Runtime. La publicación Source v3, Materialization READY/BLOCKED y pin EFFECTIVE existen; no reabrirlos al estudiar el selector de desactivación. La decisión futura debe partir del código/decisions actualizados y contar con autorización explícita.
