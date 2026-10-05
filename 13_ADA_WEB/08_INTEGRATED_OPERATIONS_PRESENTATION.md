# ADA Web — Integrated Operations Presentation

Estado: **CURRENT DESIGN CONTRACT / STATIC ALARM BASELINE IMPLEMENTED / FULL TOOL BODY PLANNED**

## Purpose

This document defines the presentation boundary for ADA Integrated Operations over CURRENT Web contracts.

It does not define a new runtime, replace ADA Generic or transfer ownership from Alarm Engine, Tool Configuration, KPI Runtime or other domains.

## Authority

```text
implementation  moragaga/atlanticus:main
decisions       moragaga/atlanticus-decisions:main
canonical       moragaga/atlanticus-cannonical:main
```

References:

```text
moragaga/isolated-web-functions:main
    Operational Trace
    responsive/visual behavior

moragaga/atlanticus-multi-stage:main
    historical Integrated Operations layout/reference
```

References do not transfer contracts, ownership or architecture.

## Ownership CURRENT

ADA Generic remains the product composition root.

CURRENT structural flow:

```text
Tool Projection
    ↓
ToolConfiguration
    ├── ToolStructure
    └── ToolRenderTopology
          ↓
OperationalRenderBinding
          ↓
external/specific composition where needed
```

Integrated Operations must not copy Generic bootstrap, Manager, identity, Navigation runtime, Tool resolution, KPI Collector or application lifecycle.

## Tool structural input

Integrated Operations consumes:

```text
ToolStructure(kind=INTEGRATED_OPERATIONS)
```

Invariants:

```text
no global operational_scope
no center_component_key
scope per component
Mine + Plant required
Mine → Plant component order
```

Component/subcomponent identities remain authoritative.

No layout roles are persisted.

## Presentation state

The specific Integrated Operations body may own one coordinated visual state:

```text
overview
mine
plant
```

This state is presentation only and must not mutate Tool Structure, KPI definitions, Alarm lifecycle or persisted business state.

## Focus semantics

`mine` and `plant` are operational focus states, not global geometric zoom.

Prefer:

```text
scope filtering
layout reflow
density changes
available-height recovery
component-aware sizing
```

Avoid global `transform: scale(...)`.

## Global Indicators

Indicator definition/state remains separate from scope placement.

Conceptual applicability must support:

```text
{MINE}
{PLANT}
{MINE, PLANT}
```

A shared indicator remains visible in both corresponding focus modes.

## Header

ADA Operational Shell remains owner of the header.

Integrated Operations may coordinate presentation of already-owned header content; it must not create a second header.

## Operational body

The specific body is materialized from `OperationalRenderBinding`.

Integrated Operations owns only its Tool-specific:

```text
Mina / Planta geometry
scope grouping
component placement
focus controls
focus reflow
Tool-specific renderers/assets
```

It does not own Generic runtime, Tool projection, KPI backend, Alarm domain rules, Manager, identity or Navigation.

## Component presentation

CURRENT capabilities have priority over historical implementations.

Keep CURRENT:

```text
Global Indicators
Time Status
Alarm Management Summary
Alarm Status
Alarm Baseline Surface
Card Display
Content State
Display Status
```

Historical ToolManifest/component-container/state-wrapper patterns remain SUPERSEDED.

## Responsive contract

Responsive and operational focus are separate axes.

Responsive reacts to available surface.

Operational focus reacts to operator context.

Historical viewport families are evidence/reference, not new frozen breakpoints.

Visual spacing, density and videowall legibility are qualified visually rather than by tests that freeze CSS structure.

## Videowall

Direction remains:

```text
videowall → overview
videowall → no normal focus controls
```

Do not infer videowall only from `width >= 2560px` without qualification.

## Alarm presentation boundary

Alarm Engine/Modeler retain ownership of:

```text
priority
lifecycle
eligibility
routing business rules
management/deactivation
dynamic scheduling
```

Web may adapt geometry/density over authoritative projections.

## Static Alarm Baseline — CURRENT

This part is now implemented.

Flow:

```text
ToolStructure + ToolRenderTopology
        ↓
OperationalRenderBinding
        ↓
AlarmBaselineProjection
        ↓
AlarmBaselineSurface
        ↓
ADA Generic
```

Integrated Operations:

```text
all components → main_points
bottom_point   → None
```

PROCESS supports a separate optional bottom point, but that topology is not an Integrated Operations role.

Current baseline anchor kind:

```text
COMPONENT
```

Component names are not rendered visibly in the static baseline.

DOM metadata preserves anchor/component identity.

## Operational Trace reference

`isolated-web-functions/operational_trace` is:

```text
SUPERSEDED as architecture
REFERENCE for visual geometry and future interaction behavior
```

The static baseline reuses its visual language for baseline/point geometry.

Not implemented from Operational Trace yet:

```text
routes
origin/affected markers
alarm cards
selection
preview
rotation
runtime tone/state
```

Those belong to a later dynamic alarm overlay and must consume authoritative alarm outputs.

## Testing boundary

Automated tests verify:

```text
valid structural projection
main/bottom split
component identity
scope inheritance
asset registration
no runtime alarm state in static surface
Generic Tool-resolution wiring
```

Automated tests should not freeze pixel spacing or CSS layout internals.

## Implementation state

CURRENT:

```text
ToolStructure contract
ToolRenderTopology
OperationalRenderBinding
static AlarmBaselineProjection
static AlarmBaselineSurface
ADA Generic baseline integration
```

PLANNED / OPEN:

```text
Integrated Operations specific operational body
overview/mine/plant focus implementation
responsive workstation qualification
videowall qualification
dynamic alarm overlay
compact alarm presentation
```

## Next focus for this track

```text
STATIC BASELINE VISUAL QUALIFICATION
```

Validate in a running ADA Generic:

```text
Integrated Operations
Process without bottom
Process with bottom
```

Only adjust presentation geometry/spacing if visual evidence requires it.

Do not open dynamic alarm behavior in the same increment.
