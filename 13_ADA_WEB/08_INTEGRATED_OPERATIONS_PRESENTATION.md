# ADA Web — Integrated Operations Presentation

Estado: **CURRENT FOUNDATION IMPLEMENTED / REAL TOOL + STATIC BASELINE BROWSER PATH VERIFIED / OPERATIONAL MODULES PLANNED**

## Purpose

This document defines the presentation boundary for ADA Integrated Operations over CURRENT Web contracts.

Integrated Operations is a permanent specialized ADA application.

It reuses ADA Generic runtime capabilities and adds Tool-specific presentation through the Generic extension contract.

It does not define a second Generic runtime or transfer ownership from Alarm Engine, Tool Configuration, KPI Runtime, Manager, identity, Navigation or KPI Collector.

## Authority

```text
implementation  moragaga/atlanticus@6ecbfb21dd0f98d7cae8f0c142d796994a7fc361
decisions       moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
canonical base  moragaga/atlanticus-cannonical@ed26b441b3055e582ba272e349f8cd6de0e067fa
```

Relevant implementation checkpoints:

```text
a334d14...  Generic additive extension contract
2ddb968...  specialized descriptor + Integrated Operations foundation
6ecbfb2...  Integrated Operations local resource harness
```

References retained:

```text
moragaga/isolated-web-functions:main
    Operational Trace visual/interaction reference

moragaga/atlanticus-multi-stage:main
    historical layout/reference only
```

References do not transfer contracts, ownership or architecture.

## Product/runtime boundary CURRENT

ADA Generic owns reusable runtime/composition:

```text
settings
durable/local persistence composition
Manager
identity
Navigation runtime
Tool Projection resolution
OperationalRenderBinding
static Alarm Baseline
KPI Collector
Master Projection
application lifecycle
resource preparation implementation
```

Integrated Operations owns:

```text
its product identity
its application extension
Dashboard product module
Mine / Plant presentation composition
Tool-specific feature modules as they are introduced
Tool-specific assets and geometry
```

Integrated Operations must not copy Generic runtime capabilities.

## Specialized application identity CURRENT

Integrated Operations uses:

```text
AdaApplicationDescriptor(
    import_name='ada.web.application.integrated_operations',
    application_id='ada-integrated-operations-application',
    display_name='ADA Operaciones Integradas',
    distribution_name='ada-integrated-operations-application',
    ...
)
```

This preserves a distinct product identity while running on Generic runtime.

## Extension boundary CURRENT

Integrated Operations supplies:

```text
create_integrated_operations_extension(
    binding: OperationalRenderBinding | None
) -> AdaApplicationExtension
```

Rules:

```text
binding is None
    → valid base/unconfigured application

binding kind == INTEGRATED_OPERATIONS
    → valid

non-null binding of another Tool kind
    → rejected
```

The extension contributes only the Dashboard application module and product page selection.

## Application-level composition CURRENT

Application root composition knows:

```text
Dashboard
```

Future siblings may be added only when they are real application-level capabilities, for example a future `restrictions` module.

The root must not know Dashboard-internal loading/haulage/cards/callbacks.

## Dashboard CURRENT

Dashboard is one `WebModule`.

It contributes:

```text
page package
Dashboard asset layer
Mine asset layer
Plant asset layer
DashboardContext service
```

Dashboard owns the current product route:

```text
/
```

There are no `/mine` or `/plant` product routes.

## Mine and Plant CURRENT

Mine and Plant are internal Dashboard compositions/layers.

They are not independent application-level `WebModule`s by default.

Current base layout:

```text
Dashboard
├── Mina
└── Planta
```

Their current layouts provide stable presentation roots and content containers.

They do not yet render the final operational component/KPI content.

## DashboardContext CURRENT

Dashboard registers an immutable service:

```text
DashboardContext(
    binding: OperationalRenderBinding | None
)
```

It can derive scoped component bindings from the authoritative Tool binding.

This avoids globals and prevents feature callbacks from resolving Tool configuration independently.

## Tool structural input CURRENT

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

## Static Alarm Baseline ownership CURRENT

The baseline capability does not live inside Integrated Operations.

Ownership:

```text
ada.web.alarms.baseline_projection
ada.web.alarms.baseline_surface
```

Generic resolves and injects it.

Flow:

```text
Tool Projection
    ↓
OperationalRenderBinding
    ├── Generic → AlarmBaselineProjection → AlarmBaselineSurface
    └── Integrated Operations → DashboardContext
```

The same binding is used by both branches.

Integrated Operations baseline contract:

```text
all components → main_points
bottom_point   → None
```

`bottom_component_key` is PROCESS-only.

Component names are not rendered visibly by the static baseline, but component/anchor/scope identity remains available through DOM metadata.

## Browser qualification CURRENT

A real Integrated Operations Tool was configured/projected and consumed by the running specialized application.

Observed:

```text
Dashboard rendered
Mina heading rendered
Planta heading rendered
static Alarm Baseline rendered
multiple baseline points rendered
baseline metadata contained configured component identities/names
```

Classification:

```text
VERIFIED
```

This closes the previous generic statement that Integrated Operations static-baseline browser qualification was entirely OPEN.

Still unverified:

```text
clean durable restart after projection without Manager changes
Process visual variants
responsive workstation behavior
videowall behavior
```

## Local durable operation CURRENT

Integrated Operations exposes:

```text
deployment/local/compose.yaml
ada-integrated-operations-resources
```

Local sequence:

```text
infra
→ product resource prepare/validate
→ application
```

`ada-integrated-operations-resources` delegates to Generic resource preparation.

The Web startup itself does not provision resources.

## KPI boundary CURRENT

Generic remains owner of KPI Collector attachment.

Future Integrated Operations features must consume Generic-delivered state/stores/adapters.

They must not connect directly to Cosmos.

Desired direction:

```text
KPI Delivery
    ↓
Generic KPI Collector
    ↓
component/store state
    ↓
Integrated Operations feature presentation
```

The first KPI-driven feature is not implemented in this closure.

## Feature-module direction CURRENT

Within Dashboard/Mine or Dashboard/Plant, functional modules may emerge such as Loading/Haulage or other operational areas.

A functional module may own:

```text
presentation
callbacks
ids
assets
feature-local state
collector/store adapters
```

It does not require its own route.

Do not introduce technical global buckets such as unrelated top-level `callbacks/`, `layouts/`, `routes/` or `components/`.

Do not create `shared/` until real cross-feature reuse exists.

Reusable capability across ADA products belongs in an appropriate generic ADA scope rather than being trapped inside Integrated Operations.

## Focus semantics PLANNED

Possible operational focus remains a presentation concern.

If introduced, `mine` and `plant` focus must not mutate Tool Structure, KPI definitions, Alarm lifecycle or persisted business state.

Prefer:

```text
scope filtering
layout reflow
density changes
available-height recovery
component-aware sizing
```

Avoid global `transform: scale(...)`.

No focus implementation is claimed by this closure.

## Global Indicators

Indicator definition/state remains separate from scope placement.

Conceptual applicability may support:

```text
{MINE}
{PLANT}
{MINE, PLANT}
```

No new runtime behavior is claimed here.

## Header

ADA Operational Shell remains owner of the header.

Integrated Operations must not create a second header.

## Dynamic alarm boundary

Alarm Engine/Modeler retain ownership of:

```text
priority
lifecycle
eligibility
routing business rules
management/deactivation
dynamic scheduling
```

Not implemented from Operational Trace:

```text
routes
origin/affected markers
alarm cards
selection
preview
rotation
runtime tone/state
```

These belong to a later dynamic alarm overlay consuming authoritative alarm outputs.

## Testing boundary

Automated tests should verify behavior/contracts such as:

```text
specialized descriptor wiring
extension/binding contract
invalid Tool kind rejection
unconfigured startup
service/context behavior
feature/store callbacks
persistence/recovery/error paths
```

Do not create tests whose only purpose is to freeze CSS geometry, exact visual structure or internal function/class existence.

Responsive/spacing/branding remain primarily visual qualification.

## Implementation state

CURRENT:

```text
Generic AdaApplicationExtension
Generic AdaApplicationDescriptor
Integrated Operations specialized application
Dashboard application module
single /
Mine + Plant internal base composition
DashboardContext
local emulator harness
product resource preparation command
real Tool → binding → Dashboard path
real Tool → binding → static baseline path
```

CLOSED for this foundation:

```text
specialized application creation
Generic reuse boundary
single Dashboard route model
Mine/Plant internal composition decision
static Alarm Baseline ownership/injection decision
product-named resource command without provisioning duplication
initial real-Tool browser proof
```

OPEN / PLANNED:

```text
durable restart proof
Tool hot reprojection refresh
first KPI-driven operational feature
actual Tool component placement/content
focus implementation
responsive workstation qualification
videowall qualification
dynamic alarm overlay
compact alarm presentation
production Azure/Entra qualification
Python 3.14.7/Trixie migration
```

## Next single focus

```text
INTEGRATED-OPERATIONS-FIRST-OPERATIONAL-DATA-VERTICAL
```

Take one real operational feature and prove:

```text
Generic KPI Collector
→ authoritative component/store state
→ one Integrated Operations feature module
→ presentation
```

The exact feature and UX approach may be selected in the next chat.

Do not mix dynamic alarms, responsive redesign or additional application-level modules into that increment.
