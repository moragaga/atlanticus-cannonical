# ADA Web — Current Baseline

Estado: **CURRENT — TOOL RENDER TOPOLOGY + STATIC ALARM BASELINE CLOSED**

## Authority

```text
Implementation  moragaga/atlanticus@686a80f6a05eeea93d35d642cf2f92100cb1e61b
Canonical base  moragaga/atlanticus-cannonical@44d3c803f60d1a1630d3a3374a663447cfe21248
```

## Tool structural ownership CURRENT

Shared structural contracts:

```text
scopes/ada-contracts/tools
ada.contracts.tools
```

Web Tool Configuration:

```text
scopes/ada/web/tools/configuration
```

CURRENT separation:

```text
ToolStructure
    domain structure
    ordered components
    PROCESS center_component_key

ToolRenderTopology
    presentation topology
    optional bottom_component_key
```

No `ProcessLayoutRole` or `layout_role` remains in the accepted contract.

## PROCESS CURRENT

```text
operational_scope       required
center_component_key    required
components              ordered
bottom_component_key    optional, outside ToolStructure
```

`center_component_key` is semantic centrality.

`bottom_component_key`:

```text
PROCESS only
existing component
different from center
```

Main row is every structure component except bottom, preserving structure order.

## INTEGRATED_OPERATIONS CURRENT

```text
no global operational_scope
no center_component_key
scope per component
Mine + Plant required
Mine → Plant order
all components in main baseline
```

## Operational Render Binding CURRENT

```text
OperationalRenderBinding
    structure
    components
    bottom_component_key
```

Derived:

```text
main_components
main_component_keys
bottom_component
```

Binding does not carry live KPI or alarm state.

## AlarmBaselineProjection CURRENT

```text
AlarmBaselineProjection
    tool_key
    kind
    main_points
    bottom_point | None
```

Point identity:

```text
anchor_kind = COMPONENT
anchor_key
component_key
display_name
scope
```

`display_name` remains contractual metadata but is not rendered visibly by the static surface.

## AlarmBaselineSurface CURRENT

Visual language was adapted from `isolated-web-functions/operational_trace`.

Retained ideas:

```text
horizontal baseline
circular point core
percentage slot-center positioning
large-screen size scaling
separate point layer semantics
```

Not ported:

```text
alarm cards
routes
origin/affected markers
preview animation
selection
runtime severity state
rotation
```

The surface retains component identity using `data-*` attributes.

## Bottom geometry CURRENT

PROCESS with bottom renders two static traces:

```text
MAIN
points positioned at centers of equal slots

BOTTOM
one full-width trace
bottom anchor centered at 50%
```

No persistent LEFT/RIGHT role is needed.

## Asset layer CURRENT

```text
ada_alarm_baseline_surface
load_order = 145
```

This avoids the existing Time Status `150` layer collision.

## ADA Generic integration CURRENT

READY Tool:

```text
Tool projection
→ render binding
→ static baseline projection
→ layout
```

No Tool:

```text
UNCONFIGURED
→ application starts
→ no default Tool
→ no baseline
```

## Qualification observed

```text
ada-web-tools-configuration             90 passed
ada-web-operational-render-binding      11 passed
ada-configuration-manager               64 passed

alarm-baseline-projection               19 passed
alarm-baseline-surface                   9 passed
ada-generic-application                206 passed

legacy ProcessLayoutRole/layout_role grep PASS
git diff --check                         PASS
```

These are focal/local test results, not full monorepo CI.

## Visual qualification

Automated tests intentionally do not freeze exact visual spacing.

OPEN:

```text
Process without bottom visual review
Process with bottom visual review
Integrated Operations visual review
spacing/density adjustment if required
responsive review
```

## Dynamic alarm boundary

OPEN / SEPARATE:

```text
alarm-live-projection consumption
runtime route overlay
origin/affected markers
alarm cards
preview/selection
dynamic state colors
management interaction
```

The Web layer must consume authoritative runtime/modeler outputs and must not recreate alarm business rules.

## Existing global conflict

Project target remains Python 3.14.7 while multiple package metadata entries still declare 3.14.2.

This hito does not resolve that conflict.
