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
