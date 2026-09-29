# ADA Command Center — Alarm Configuration Authoring Model

Estado: **CURRENT — Source Snapshot v3, editor guiado, routing estricto; C1 Tool services/UI Web CLOSED estructuralmente; deactivation fin de turno y aceptación browser OPEN**. Corte histórico de este frente 2026-09-28 `atlanticus@a799dc15105d3e037f36ab77129ef0cfa8999013`; delta de ownership verificado `atlanticus:main@3961385aecd0eb7e373018fc25e509a71dccc409`. Los gates previos de componente NO equivalen a aceptación global de host/browser.

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

Familia derivada de `AlarmIdentity.family_key` y Messages `scope=FAMILY`, sin entidad Family separada. `alarm_key` estable; no sustituirlo con `rule_name`/display/title. Cn deriva del manifest Tool congelado junto con Rn. Source v2 SUPERSEDED, sin decoder legacy actual. Conservar referencias de Rules activas/inactivas, origen, steps habilitados/deshabilitados, visual targets y estructura Tool exacta para reconstrucción sin latest.

## Workspace, Save, Validate, Publish — CURRENT

El editor trabaja con `AlarmConfiguration` y sidecar `_confirmed_tool_catalog_revision` fuera del agregado. Save Draft fija Cn; Validate comprueba configuración intrínseca, pin actual y referencias Tool; Publish vuelve a comprobar y congela subset exacto en Source v3. Si Cn cambia, se necesita acción deliberada. Releases Rold/Cold inmutables. `VALID_AT_SAVE != B.2 READY != EFFECTIVE`; no publicar un borrador incompleto silenciosamente.

El editor guiado gestiona familias, Rules/Messages, selección Tool/Component/Subcomponent y routing. C1 migró el servicio/UI del Tool Catalog confirmado a bibliotecas Web independientes (`web/tools/catalog`, `web/tools/discovery-cosmos`, `web/tools/catalog-manager`); esto no modifica los contratos de authoring ni cierra aceptación visual/host/browser.

## Routing FROZEN; presentación OPEN

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END
```

No saltar, repetir nivel ni retroceder. C1/C2 pueden permanecer en origen; C3 sólo origen. C1 routing inmediato y C2 espera positiva con offsets B.2. Web reutiliza `next_routing_tool_kind`. Configuración histórica incompatible queda visible para corrección, sin borrado automático ni cambio silencioso de criticality.

`routing_tools` puede incluir STRATEGIC; `tools` para visual tiene PROCESS e INTEGRATED_OPERATIONS. Visual targets por Rule contienen Tool, `component_keys`, subcomponentes `(owner_component_key,subcomponent_key)` y `process_projection_mode` sólo para Process. `QUEUE_IN_QUEUE` de Integrated Operations y `CAROUSEL` de Process son estrategias visuales futuras, no campos Source v3 ni scheduler ya programado. Sigue OPEN/CONFLICT la independencia conceptual de targets visuales respecto a la sincronización actual desde routing; ver `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md`.

## Parametrización del evaluator — precisión B2c.5d

Rules conservan `evaluator_key` y `parameters: Mapping[str,str|float|bool]` sin `None`/listas/dicts anidados/expresiones. Los parámetros de negocio son opcionales: Web no debe imponer un `limit`/`factor` global ni derivar automáticamente columnas, particiones y fuentes de datos. Desarrollador de evaluador define sus parámetros y requisitos de datos. Ejemplo controlado B2c.5d con `limit` default 80.0 es una DEMO, no Rule/evaluator registrado en producción.

`(family_key,evaluator_key)` resuelve implementación; `alarm_key` identifica la Rule. `EvidenceSnapshot` es resultado flexible del backend evaluator, no autorización para que la Web de configuración decida lifecycle/WAL/evidencia.

## OPEN — máximo de desactivación hasta fin de turno

**CURRENT:** `default_deactivation.max_duration_hours` usa `_number_field`; Domain `AlarmDeactivationDefinition`/`MessageDeactivationDefinition` aceptan `max_duration_hours: int|None` y, al habilitar desactivación, exigen entero **1..12**. No hay selector contractual confirmado para límite «fin del turno». Que Core maneje `effective_until` UTC no implementa esa capacidad de authoring.

Antes de tocar código decidir semántica exacta, turno Mine/Plant, timezone/calendario, aprobación, overrides Message y compatibilidad con 1..12. No introducir campos/enums/algoritmos/migraciones sin decisión de este frente independiente.

## Qualification B1d / cierre de ownership C1

B1d verificó catálogo confirmado y aparición de Tools en Manager más una Alarm Source `local`, NO Alarm Source Blob/Projection Cosmos durable. La publicación y la proyección siguen siendo acciones distintas. C1 cerró la extracción UI a `web/tools/catalog-manager` y traslado de servicios a Web; pruebas/wheels/importaciones locales GREEN pero browser/UX siguen UNVERIFIED.

**OPEN / frente Web separado:** decidir «fin de turno» y cerrar modal sólo tras guardar con éxito; mostrar errores sin cerrarlo cuando falla. El Starter distribuible aún está PLANNED. Estos trabajos NO son el siguiente foco C2 ni están autorizados por el cierre C1.

## Frontera siguiente del traspaso

La implementación Materialization READY/BLOCKED y los contratos EFFECTIVE exactos existen antes de C1. El foco siguiente C2 es sólo APPLICATION/rutas/Source Key y contenedor Cosmos según contrato para los tres procesos. No reabrir source v3, routing o presentación visual ni mezclar producer Qualification/Live Delivery durante ese incremento.
