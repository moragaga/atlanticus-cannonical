# ADA Command Center — Configuration Scope

Estado: **CURRENT / ALARM CONFIGURATION SNAPSHOT V3 IMPLEMENTED**

Command Center owns Alarm Configuration administration.

It reuses `atlanticus.web.manager`; the Manager generic remains unchanged.

## Editable aggregate

```text
AlarmConfiguration
    rules
    messages
```

The editor does not embed Tool definitions.

## Durable published aggregate

```text
AlarmConfigurationSnapshot
    configuration: AlarmConfiguration
    tool_dependencies: ToolDependencyManifest
```

This is the CURRENT durable Alarm source payload.

The earlier statement that Tool Catalog metadata did not change the durable Alarm snapshot is
SUPERSEDED.

## Tool correlation

Workspace-specific metadata:

```text
_confirmed_tool_catalog_revision
```

is not part of `AlarmConfiguration`.

## Save/validate/publish

```text
Save Draft
-> read current Confirmed Tool Catalog
-> pin Cn in workspace

Validate
-> intrinsic AlarmConfiguration validation
-> require pinned Cn == current Cn
-> require every referenced Tool key exists

Verify Source
-> generic Manager source concurrency

Publish
-> require Cn still current
-> select referenced ToolDependencyEntries
-> persist AlarmConfigurationSnapshot v3
```

No in-memory validation cache is required.

## Drift behavior

```text
saved workspace C1
Tools becomes C2
publish without new save/validation
-> rejected
```

Existing durable `R1/C1` remains unchanged.

## Tool references persisted

Includes Tools referenced by:
- origin;
- every escalation step, including disabled;
- every visual target;
- every Rule, including inactive.

## Authoring UI

UI read model continues to expose Tool/Component/Subcomponent suggestions.

STRATEGIC is excluded from Alarm authoring suggestions, but the full dependency catalog derived from
the same Tool snapshot retains every Tool entry.

## Projection base

```text
Source release
-> AlarmConfigurationProjectionBuilder
-> ProjectionRecord[AlarmConfigurationSnapshot]
```

No Tool reread.

## Estado posterior y foco actual

En `atlanticus:main@7b61eaea463bab10a595166fa12d015e4c015c78` existen adapters
local/Cosmos, stores de Source local/Blob y composición de persistencia de Alarm Configuration.
Esto **reemplaza** la afirmación histórica de que todavía faltaba crear el adapter Cosmos,
pero no demuestra un despliegue Azure end-to-end.

La prioridad inmediata es cerrar la UX del Manager. Ver
`18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md`; Materialization y Live Delivery son
frentes posteriores y no se implementan aquí.
