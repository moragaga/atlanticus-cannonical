# ADA Command Center — Alarm Configuration Authoring Model

Estado: **CURRENT / SNAPSHOT V3 + GUIDED EDITOR + STRICT ROUTING IMPLEMENTED; MATERIALIZATION JOB PLANNED**

Checkpoint auditado: `moragaga/atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b`.

## Authored aggregate, sin metadata de workspace

```text
AlarmConfiguration
  rules
  messages
```

La familia es una agrupación derivada de `AlarmIdentity.family_key` y de Messages `scope=FAMILY`; no es una entidad persistente separada. Mensajes `GLOBAL` se gestionan por separado. La familia nueva queda durable con su primera Rule o Message.

`alarm_key` identifica de forma estable una alarma; `rule_name` y `display_name` no reemplazan esa identidad.

## Durable release y evidencia exacta

```text
AlarmConfigurationSnapshot
  configuration: AlarmConfiguration
  tool_dependencies: ToolDependencyManifest

schema_version = 3
```

`confirmed_tool_catalog_revision` se deriva del manifest. v2 **SUPERSEDED** y sin compat decoder.

Las referencias que se congelan incluyen Rules inactivas, origin Tool, steps habilitados/deshabilitados y visual targets. Cada entry congela `tool_key`, display name, source release id, kind y `ToolStructure` completo para reconstruir la evidencia sin nuevas lecturas al Tool Catalog.

## Workspace/validate/publish CURRENT

El editor opera sobre `AlarmConfiguration`; el sidecar del workspace `_confirmed_tool_catalog_revision` no es parte del agregado.

```text
Save Draft pins Cn
Validate: intrinsic Alarm + pinned Cn current + defined Tool references exist
Publish: repeats same Cn check, freezes exact subset and persists Source v3
```

Un cambio `Cn -> Cn+1` obliga a nueva acción deliberada para adoptar la revisión actual; releases históricas Rold/Cold no cambian. El Manager genérico permanece independiente del contrato Alarm específico.

```text
VALID_AT_SAVE != B.2 READY != EFFECTIVE
```

Los borradores transitoriamente incompletos, su persistencia, diagnóstico y recuperación no deben confundirse con candidatos publicables.

## Guided editor CURRENT / VERIFIED A NIVEL COMPONENTE

En este hito se implementaron y ejercitaron mediante suites del componente: familias derivadas, tarjetas consistentes, editor de reglas/mensajes, ranking/grupo visible de reglas, corrección del input Nueva familia y selección de routing guiada. La verificación real host/browser completa tras el routing sigue **UNVERIFIED**; no elevar las suites de componentes a aceptación visual/E2E.

## Routing FROZEN

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END
```

No saltar nivel, permanecer en el mismo ni retroceder. C1/C2 pueden no tener destinos; C3 origen únicamente. C1 pasos inmediatos y C2 esperas positivas acumuladas en B.2. La Web comparte `next_routing_tool_kind` con Domain y B.2 valida contra la evidencia Tool exacta. La configuración anterior inválida se mantiene visible para corregirse; ninguna autoeliminación ni auto-cambio de criticidad.

El documento de referencias Web distingue `routing_tools` (incluye Strategic) de `tools` con proyección visual (Process/Integrated Operations). Strategic no permite visual targets hasta que exista contrato explícito.

## Visual targets y contrato futuro de UI

Cada Rule conserva `visual_targets` con Tool, `component_keys`, `subcomponents` identificados por `(owner_component_key, subcomponent_key)` y `process_projection_mode` exclusivo de Process. `QUEUE_IN_QUEUE` para Integrated Operations y `CAROUSEL` para Process son estrategias de presentación futuras derivadas de Tool kind, **no campos nuevos** del Source v3 ni scheduler implementado. El contrato detallado permanece en `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md`.

**CONFLICT:** ese documento establece que visual targets no equivalen a destinos de routing; el editor CURRENT sincroniza visual targets desde origin y routing habilitado (excluyendo Strategic). No resolver implícitamente cambiando el schema ni el job.

## Frontera siguiente

Source codec, builder, stores Local/Cosmos y provider composition de proyección ya existen. El siguiente chat debe auditar el input operacional exacto y diseñar/implementar **sólo** el job de Materialization que invoca el pure B.2 resolver y deja los artefactos disponibles para Runtime. Infraestructura real, qualifications operacionales, artifact stores y E2E permanecen OPEN.
