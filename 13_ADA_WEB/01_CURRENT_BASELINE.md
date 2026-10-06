# ADA Web — Current Baseline

Estado: **CURRENT — INTEGRATED OPERATIONS INITIAL UI IMPLEMENTED / FINAL POLISH VISUAL RECHECK OPEN**

## Authority

```text
Implementation  moragaga/atlanticus@eec22faa9cca5ad67af8e5fe0bf0299264e27ad5
Canonical base  moragaga/atlanticus-cannonical@c1c5479930ebbde45d3f4b636ba6f1d72f7db13b
Decisions       moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
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

## INTEGRATED_OPERATIONS structural contract CURRENT

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

Generic resolves this binding once and provides the same object to Generic presentation and the specialized application extension.

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

The static surface retains component identity with DOM metadata such as:

```text
data-ada-alarm-anchor-key
data-ada-component-key
data-ada-scope
```

`display_name` remains metadata and is not rendered visibly by the baseline surface.

## AlarmBaselineSurface ownership CURRENT

Ownership remains:

```text
ada.web.alarms.baseline_projection
ada.web.alarms.baseline_surface
```

Generic composes/injects the baseline.

Integrated Operations does not recreate the projection and does not own Alarm Engine business rules.

## Bottom geometry CURRENT

PROCESS with bottom renders:

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
→ Generic layout
→ specialized extension uses same binding
```

No Tool:

```text
UNCONFIGURED
→ application starts
→ no default Tool
→ no static baseline
```

## Specialized application contract CURRENT

ADA Generic supports:

```text
AdaApplicationDescriptor
AdaApplicationExtension
```

The current specialized consumer is:

```text
ada-integrated-operations-application
```

It reuses Generic runtime and contributes its product-specific Dashboard composition.

## Integrated Operations Dashboard CURRENT

Application-level composition knows only:

```text
Dashboard
```

Current route:

```text
/
```

Mine and Plant are internal Dashboard composition layers, not product routes.

Current static visual inventory:

```text
9 operational component columns
22 visual cards
```

Component inventory:

```text
MINE
    general_mina
    carguio
    transporte
    chancado_stmg

PLANT
    stockpile_chacay
    molienda
    flotacion
    transporte_fluidos
    puerto
```

`gestion_carguio_turno` is a shared visual card spanning Carguío and Transporte; it is not a tenth Tool component.

## Dashboard binding boundary CURRENT

Product-local bindings live in:

```text
ada.web.application.integrated_operations.modules.dashboard.bindings
```

Current distinction:

```text
visual key / label
    CURRENT presentation identity

tool_component_key
tool_subcomponent_key
linked_tool_component_keys
    optional Tool identity mapping
```

The current built-in visual bindings do not yet provide authoritative Tool component/subcomponent keys.

Therefore:

```text
visual keys MUST NOT be assumed to equal Tool keys
historical/reference names MUST NOT be silently promoted to Tool identity
```

When Tool IDs are supplied, the card/component builders project them to DOM metadata without requiring layout changes.

## Card presentation boundary CURRENT

Product-local builders currently exist in:

```text
ada.web.application.integrated_operations.modules.dashboard.card
```

They build:

```text
component panel
regular dashboard card
shared dashboard card
```

This shell is CURRENT implementation.

Whether this should migrate to or reuse an existing capability under `scopes/ada/web/ui` is not decided.

Classification:

```text
OPEN / PLANNED design review
```

## Desktop overview geometry CURRENT

Desktop and videowall overview use one master grid:

```text
9 equal tracks
4 Mine
5 Plant
```

A single dashboard gap token is used between scope/component tracks.

Mine internally uses four equal columns and two rows.

Plant internally uses five equal columns.

The transverse Carguío/Transporte shared card occupies Mine columns 2-3 on the second row.

## Focus semantics CURRENT

Focus is a presentation concern only.

Current desktop behavior:

```text
overview
    Mine + Plant visible

mine
    Plant hidden
    Mine occupies full presentation width

plant
    Mine hidden
    Plant occupies full presentation width
```

The implementation does not mutate Tool Structure, KPI definitions, alarm lifecycle or persisted state.

The previous width-expansion/translation technique was superseded.

No global `transform: scale(...)` is used.

## Responsive contract CURRENT

### Mobile `<1280`

```text
Mine + Plant both rendered vertically
components rendered vertically
cards rendered vertically
no Mine/Plant presentation controls
Alarm Management slot hidden
Alarm Status slot hidden
Alarm Baseline hidden
```

This behavior is CURRENT specifically in Integrated Operations CSS.

It is not yet a generic ADA-wide reusable contract.

### Tablet `1280-1365`

```text
one scope visible at a time
new/invalid/overview state resolves to Mine
when Mine is visible → only PLANTA control remains
when Plant is visible → only MINA control remains
focused baseline points use only the visible scope
visible baseline points are redistributed over full width
```

### Desktop `1366-2559`

```text
overview = 9-track full layout
Mine/Plant controls mounted visually on Alarm Baseline
focus hides opposite scope
focus filters/repositions static baseline to visible scope
close control returns to overview
```

### Videowall `>=2560`

```text
overview forced
Mine + Plant visible
presentation controls hidden
```

## Integrated Operations / Alarm Baseline presentation coordination CURRENT

The static baseline remains Generic-owned.

Integrated Operations presentation JS currently:

```text
finds the Generic AlarmBaselineSurface in the same application
mounts the Mine/Plant control wrapper into the baseline surface
filters points by existing data-ada-scope metadata while focused
temporarily rewrites --ada-alarm-baseline-point-x for visible points
restores original point positions when returning to overview
```

This is a CURRENT product presentation technique.

It is not a new Alarm Engine/domain contract and must not be generalized without a separate decision.

## Initial browser evidence

During this hito the running specialized application was observed with:

```text
full-width Dashboard
9-column overview
22 visual cards
Mine/Plant focus
responsive layout iterations
static Alarm Baseline
focus-aware baseline behavior
```

The last implementation checkpoint `eec22faa...` changed only final CSS presentation polish after the preceding focus/responsive checkpoint.

Classification:

```text
implemented in main                               VERIFIED
browser behavior before final CSS polish          VERIFIED
exact final eec22faa visual requalification       UNVERIFIED / OPEN
```

## Automated qualification boundary

Current package tests assert behavior/contracts including:

```text
9 dashboard component declarations
22 unique visual cards
Mine + Plant scope metadata
overview/mine/plant presentation targets
Tool identity projection when bindings are supplied
shared owner/linked identity projection
presentation JS packaged
```

A local run earlier in the hito reported:

```text
5 passed
```

No clean rerun against the exact final `eec22faa...` checkpoint was captured in this closure.

Status:

```text
test definitions CURRENT
exact final-head rerun UNVERIFIED / OPEN
```

Automated tests intentionally do not freeze exact CSS geometry.

## KPI boundary CURRENT

Generic remains owner of KPI Collector attachment.

Integrated Operations cards do not yet consume KPI state.

Required future flow remains:

```text
KPI Delivery
→ Generic KPI Collector
→ authoritative component/store state
→ Integrated Operations feature/card presentation
```

Integrated Operations must not connect directly to Cosmos.

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

Project target remains Python 3.14.7 while package metadata still declares:

```text
requires-python = ==3.14.2
```

for `ada-integrated-operations-application`.

This hito does not resolve that conflict.

## Qualification debt remaining

```text
exact browser recheck of final eec22faa CSS polish
clean package test rerun on final head
ruff format clean proof on final head
durable restart proof without Manager intervention
Process static baseline visual variants
```

These items do not reopen the initial Integrated Operations UI scope.

The initial UI foundation is considered:

```text
CLOSED
```

with the qualification debt above remaining explicit.
