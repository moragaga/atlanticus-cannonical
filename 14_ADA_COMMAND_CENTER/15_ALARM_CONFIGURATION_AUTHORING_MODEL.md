# ADA Command Center — Alarm Configuration Authoring Model

Estado: **CURRENT / AUTHORED AGGREGATE + VERSIONED SNAPSHOT V3 IMPLEMENTED**

## Authored aggregate

```text
AlarmConfiguration
    rules
    messages
```

This remains free of workspace metadata.

## Durable versioned snapshot

```text
AlarmConfigurationSnapshot
    configuration
    tool_dependencies: ToolDependencyManifest
```

Tool evidence is snapshot metadata, not part of `AlarmConfiguration`.

## Source schema

```text
schema_version = 3
```

v2 is SUPERSEDED.
No compatibility decoder.

## Tool manifest

Every selected Tool stores:

```text
tool_key
display_name
source_release_id
kind
ToolStructure
```

Full ToolStructure retains Component/Subcomponent display names and relationships for history and B.2.

## Referenced Tool set

Capture includes:
- inactive Rules;
- origin Tool;
- enabled and disabled escalation steps;
- all visual targets.

No defined reference disappears because it is non-executable now.

## Workspace Tool pin

```text
_confirmed_tool_catalog_revision
```

UI editor displays only `AlarmConfiguration`.

The sidecar participates in Manager workspace revision but is not persisted into the authored
aggregate.

## Validation layers

### Intrinsic

`AlarmConfiguration.from_document()` owns aggregate invariants.

### Alarm Manager Tool correlation

Before publication:
- require current Confirmed Tool Catalog;
- require pinned revision still current;
- require all referenced Tool keys exist.

### B.2

Later:
- evaluator qualification;
- Tool GREEN qualification;
- routing;
- visual target semantics;
- Runtime/Delivery resolution.

Therefore:

```text
VALID_AT_SAVE != READY != EFFECTIVE
```

## Drift

```text
workspace C1
current Tools C2
```

Validation fails until user saves the draft again against C2.

Existing durable `Rold/Cold` is unaffected.

## New publication

If Rules/Messages are unchanged but a new Tool revision is intentionally adopted, Source bytes still
change because manifest/revision changes.

## Manager boundary

Do not add Alarm dependency metadata to generic Manager.

Alarm-specific semantics remain in:
- workspace binding;
- validation workflow;
- source workflow.

## Manager UX y presentación visual — decisión posterior

El agregado `rules + messages` y el schema v3 se mantienen. Las familias son agrupaciones
derivadas de `AlarmIdentity.family_key` y del scope de Messages; no se agrega `Family` durable.

La configuración authored de cada Rule conserva `visual_targets` con Tool, Components,
Subcomponents `(owner_component_key, subcomponent_key)` y `process_projection_mode` sólo para
Process. El tipo de presentación futuro se deduce del Tool kind: Integrated Operations
`QUEUE_IN_QUEUE`; Process `CAROUSEL`. **No introducir todavía estos nombres como campos nuevos**
del snapshot. Las reglas de rotación futura y el plan de cierre del editor se registran en
`18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md`.

El Manager UX y las mejoras de draft recuperable/diagnóstico siguen IN PROGRESS/OPEN; su
aceptación no se deduce de la existencia del contrato v3.
