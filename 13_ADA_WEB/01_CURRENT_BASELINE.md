# ADA Web — Current Baseline

Estado: **CURRENT — SPECIALIZED INTEGRATED OPERATIONS FOUNDATION + STATIC ALARM BASELINE VERIFIED**

## Authority

```text
Implementation  moragaga/atlanticus@6ecbfb21dd0f98d7cae8f0c142d796994a7fc361
Canonical base  moragaga/atlanticus-cannonical@ed26b441b3055e582ba272e349f8cd6de0e067fa
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

For specialized applications, Generic resolves this binding once and can provide the same object to both Generic presentation and the application extension.

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

The surface retains component identity using `data-*` attributes.

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

## Specialized application contract CURRENT

ADA Generic now supports:

```text
AdaApplicationDescriptor
AdaApplicationExtension
```

The current specialized consumer is:

```text
ada-integrated-operations-application
```

It uses Generic runtime and contributes only its product-specific Dashboard composition.

## Integrated Operations browser evidence

Observed in a running durable Integrated Operations application:

```text
real projected Tool loaded
Dashboard rendered at /
Mina surface visible
Planta surface visible
static baseline visible
baseline points correspond to configured Tool component identities/names
```

Classification:

```text
VERIFIED / CURRENT
```

A separate durable restart proof without touching Manager was requested but not reported before closure.

Classification:

```text
UNVERIFIED / OPEN
```

## Qualification boundary

Automated tests intentionally do not freeze exact visual spacing.

Still OPEN:

```text
Process without bottom visual review
Process with bottom visual review
Integrated Operations responsive review
videowall qualification
spacing/density adjustment if evidence requires it
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

Project target remains Python 3.14.7 while multiple package metadata entries still declare 3.14.2, including Integrated Operations.

This hito does not resolve that conflict.
