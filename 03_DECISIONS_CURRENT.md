# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global — FROZEN

```text
uv; no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
one focus per increment
Git read-only unless explicit authorization
```

## Tool Structure — CURRENT / FROZEN

`ToolStructure.components` is the authoritative operational sequence.

Persistent layout roles are removed.

```text
ProcessLayoutRole        SUPERSEDED
layout_role              SUPERSEDED
LEFT/CENTER/RIGHT roles  SUPERSEDED
```

PROCESS uses:

```text
operational_scope
center_component_key
ordered components
```

`center_component_key` is semantic centrality, not a physical screen coordinate.

INTEGRATED_OPERATIONS uses component scopes and Mine → Plant operational order.

## Render Topology — CURRENT / FROZEN

Presentation topology is separate from `ToolStructure`.

CURRENT contract:

```text
ToolRenderTopology(
    bottom_component_key: str | None
)
```

Rules:

```text
optional
PROCESS only
must reference an existing component
must differ from center_component_key
```

A missing/empty topology is serialized compatibly by omitting `render_topology`.

Do not reintroduce permanent LEFT/RIGHT/BOTTOM domain roles.

## Operational Render Binding — REFINED / CURRENT

Previous interpretation:

```text
binding = ToolStructure + ordered components only
```

CURRENT:

```text
binding =
    ToolStructure
    + ordered component bindings
    + optional bottom_component_key
```

Derived topology:

```text
main_components = components excluding bottom
bottom_component = configured bottom or None
```

It still contains no KPI runtime data and no live alarm state.

## Static Alarm Baseline — CURRENT / CLOSED

Previous Process behavior:

```text
project only center component
```

is SUPERSEDED.

CURRENT projection:

```text
AlarmBaselineProjection(
    tool_key,
    kind,
    main_points,
    bottom_point | None,
)
```

For PROCESS:

```text
all Tool components participate
bottom, if configured, is separated from main_points
main order follows ToolStructure.components
```

For INTEGRATED_OPERATIONS:

```text
all components remain in main_points
```

Current static anchors are COMPONENT anchors only.

The baseline does not own alarm lifecycle, severity, routes, cards, selection or preview.

## Static Alarm Surface — CURRENT / CLOSED

`AlarmBaselineSurface` uses the proven visual language of `isolated-web-functions/operational_trace` for:

```text
baseline
point core
slot-centered percentage positioning
large-screen sizing
```

but does not import its architecture or dynamic alarm model.

`isolated-web-functions` is REFERENCE, not authority.

Visible component names are intentionally omitted from the baseline surface.

Component identity remains in DOM metadata for future overlay.

Asset order:

```text
Alarm Management Summary  130
Alarm Status              140
Alarm Baseline Surface    145
Time Status               150
```

## ADA Generic Tool resolution — CURRENT

READY:

```text
Tool Projection
→ ToolStructure + ToolRenderTopology
→ OperationalRenderBinding
→ AlarmBaselineProjection
→ application definition/layout
```

UNCONFIGURED / UNAVAILABLE / INVALID continue through the degraded/base application path according to existing resolution semantics.

No Tool configured means:

```text
application can start
no implicit/default Tool
no static alarm baseline
```

## Dynamic alarms — SEPARATE / PLANNED

The static baseline closure does not implement:

```text
alarm-live-projection consumption
runtime routes
origin/affected overlay
alarm cards
preview
selection
dynamic colors
management
history/analytics
```

Those must consume Alarm Engine/Modeler outputs rather than recreate domain logic in Web.

## Alarm pipeline — CURRENT / CLOSED baseline

The implemented backend pipeline remains:

```text
Command Center publication
    ↓
Alarm Configuration projection
    ↓
Materialization READY
    ↓
Runtime EFFECTIVE
    ↓
Runtime CURRENT + FACTS
    ↓
Modeler current projection
    ↓
Delivery
    ↓
Cosmos alarm-live-projection
    ↓
Web dynamic consumer [SEPARATE]
```

The static baseline implemented in ADA Generic is structural and does not replace the dynamic consumer.

## Exact artifact invariant — FROZEN

```text
READY != EFFECTIVE
exact artifact = source_key + result_id + manifest_sha256 + resolution_key
Runtime, Modeler y Delivery usan el mismo exact artifact
no fallback to latest READY
```

## Materialization split — REFINED

CURRENT:

```text
RuntimeAlarmConfiguration + DeliveryAlarmConfiguration
```

Modeler baseline consumes both from the same READY exact artifact.

A separate `ModelerConfiguration` is not a prerequisite and should appear only if an independent responsibility is demonstrated.

## Runtime ownership — FROZEN

Runtime owns evaluation, occurrence/episode, priority truth, operational management effects, assignments/routing state, EFFECTIVE adoption and durable operational facts.

Runtime does not own Web geometry.

## Modeler ownership — CURRENT baseline / advanced scheduling PLANNED

CURRENT:

```text
consume authoritative Runtime CURRENT
reopen exact materialized configuration
filter eligible PREDOMINANT active alarms
build per-Tool operator_pool
build first-six operator_view
persist current index + per-Tool snapshot
validate checksums/current exact pin
```

PLANNED:

```text
CAROUSEL
QUEUE_IN_QUEUE
rotation timers
fairness
durable scheduler checkpoint/state
staleness/disconnection policy
```

## Delivery — CURRENT / FROZEN

Delivery owns Tool → Cosmos connection resolution, bounded publication and transport/upsert.

It does not own UI geometry or alarm modeling.

## Cause — OPEN

CURRENT snapshot retains `cause_template` plus evidence.

Dynamic effective cause materialization remains OPEN.

## CAROUSEL / QIQ — NOT IMPLEMENTED

Prior design knowledge remains reference/partial design only until scheduler state and qualification exist.
