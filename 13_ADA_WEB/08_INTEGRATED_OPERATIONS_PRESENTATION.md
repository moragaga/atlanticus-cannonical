# ADA Web — Integrated Operations Presentation

Estado: **CURRENT INITIAL UI IMPLEMENTED / TOOL ID + REUSE BOUNDARY OPEN / KPI CONTENT PLANNED**

## Purpose

This document defines the presentation boundary for ADA Integrated Operations over CURRENT Web contracts.

Integrated Operations is a permanent specialized ADA application.

It reuses ADA Generic runtime capabilities and adds Tool-specific presentation through the Generic extension contract.

It does not define a second Generic runtime or transfer ownership from Alarm Engine, Tool Configuration, KPI Runtime, Manager, identity, Navigation or KPI Collector.

## Authority

```text
implementation  moragaga/atlanticus@eec22faa9cca5ad67af8e5fe0bf0299264e27ad5
decisions       moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
canonical base  moragaga/atlanticus-cannonical@c1c5479930ebbde45d3f4b636ba6f1d72f7db13b
```

Relevant implementation checkpoints:

```text
a334d14...  Generic additive extension contract
2ddb968...  specialized descriptor + Integrated Operations foundation
6ecbfb2...  local resource harness
ed1d1af...  complete initial 9-component / 22-card Dashboard
f16ce93...  final focus/responsive/baseline coordination structure
eec22fa...  current final CSS polish
```

References retained:

```text
moragaga/isolated-web-functions:main
    alarm/component visual correlation reference

moragaga/__temporal_ada_latest:main
    historical/current ADA layout reference used for geometry/content inventory

moragaga/atlanticus-multi-stage:main
    historical 4+5 layout/focus reference
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
static Alarm Baseline projection/module injection
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
product-local visual layout
product-local presentation assets
future Tool-specific feature presentation
```

Integrated Operations must not copy Generic runtime capabilities.

## Specialized application identity CURRENT

Integrated Operations uses its own `AdaApplicationDescriptor` while running on Generic runtime.

This preserves distinct product identity without replacing Generic contracts.

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

## Application-level composition CURRENT

Application root composition knows only:

```text
Dashboard
```

Current product route:

```text
/
```

There are no `/mine` or `/plant` product routes.

Mine and Plant are internal Dashboard compositions.

## DashboardContext CURRENT

Dashboard registers:

```text
DashboardContext(
    binding: OperationalRenderBinding | None
)
```

It can derive scoped component bindings from the authoritative Tool binding.

Feature code must not independently resolve Tool configuration.

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

## Complete initial Dashboard layout CURRENT

The current Dashboard no longer consists of empty Mine/Plant containers.

It renders a complete initial static presentation inventory.

Master overview:

```text
9 equal tracks
├── 4 Mine tracks
└── 5 Plant tracks
```

MINE component columns:

```text
general_mina
carguio
transporte
chancado_stmg
```

PLANT component columns:

```text
stockpile_chacay
molienda
flotacion
transporte_fluidos
puerto
```

The dashboard declares 22 visual cards in total.

The shared card:

```text
gestion_carguio_turno
```

spans the Carguío + Transporte area but is not a tenth Tool component.

## Visual card inventory CURRENT

```text
GENERAL MINA
    Movimiento Mina
    Remanentes
    Perforación
    MP10

CARGUÍO
    Equipos de Servicio
    Mezcla hacia Chancado

TRANSPORTE
    Transporte Global • Turno
    N° Operativo • Turno
    Tiempos y Colas • Turno

SHARED CARGUÍO / TRANSPORTE
    Gestión Carguío • Turno

CHANCADO-STMG
    Chancado-STMG

STOCKPILE CHACAY
    Stockpile Chacay
    Tendencia Alimentado

MOLIENDA
    Molienda

FLOTACIÓN
    Colectiva
    Selectiva

TRANSPORTE DE FLUIDOS
    STR
    STC
    Tranque
    STA

PUERTO
    Puerto
    Desaladora
```

These are presentation labels/slots, not authoritative Tool IDs.

## Product-local binding contract CURRENT

Current binding types:

```text
DashboardCardBinding
DashboardComponentBinding
DashboardSharedCardBinding
```

Current product mapping file:

```text
ada.web.application.integrated_operations.modules.dashboard.bindings
```

Important distinction:

```text
key
label
    visual/product identity

tool_component_key
tool_subcomponent_key
linked_tool_component_keys
    authoritative Tool identity references when explicitly supplied
```

The built-in visual bindings currently leave Tool identity fields unset.

Therefore:

```text
DO NOT infer Tool IDs from visual keys
DO NOT copy historical IDs from reference repositories as authority
DO NOT bind KPI/alarm data to a card until the real Tool key is explicitly mapped
```

## Developer intervention point OPEN

The developer/user must eventually populate the real Tool identity mapping.

The intended intervention point is:

```text
modules/dashboard/bindings.py
```

When supplied, current builders propagate identity as DOM metadata:

```text
data-ada-component-key
data-ada-subcomponent-key
data-ada-linked-component-keys
```

The exact real Tool IDs were not available/validated in this hito.

Status:

```text
PLANNED / OPEN
```

## Product-local card shell CURRENT

Current implementation contains:

```text
modules/dashboard/card.py
```

with builders for:

```text
component panel
regular dashboard card
shared dashboard card
```

This implementation is CURRENT because it is in `atlanticus:main`.

However, a possible existing reusable card capability under:

```text
scopes/ada/web/ui
```

was identified by the user at closure.

This overlap was not evaluated in this hito.

Status:

```text
OPEN
```

Before additional ADA consumers depend on the product-local shell, compare the existing reusable UI capability and decide one of:

```text
reuse existing shared ADA card
promote/migrate the new shell into the appropriate ADA UI scope
retain product-local shell because contracts materially differ
```

Do not create adapters or duplicate both implementations by default.

## Geometry CURRENT

The overview uses a single nine-track grid so all operational component columns share the same width.

A single product token controls inter-column/inter-card spacing:

```text
--ada-io-gap
```

Mine internally uses:

```text
4 equal columns
2 equal rows
```

The shared Carguío/Transporte card spans:

```text
columns 2-3
row 2
```

Plant internally uses:

```text
5 equal columns
```

The Dashboard has a small product-local edge inset and left/right/bottom border.

## Focus CURRENT

Focus is presentation state only:

```text
overview
mine
plant
```

Desktop behavior:

```text
overview
    both scopes visible

mine
    Plant hidden
    Mine spans full dashboard width

plant
    Mine hidden
    Plant spans full dashboard width
```

Closing focus returns to `overview`.

The current implementation does not use global `transform: scale(...)`.

The earlier `225%/180% + translateX` approach is SUPERSEDED.

## Responsive modes CURRENT

### Mobile `<1280`

```text
Mine + Plant both visible
all scopes/components/cards stacked vertically
no Mine/Plant focus controls
Alarm Management hidden by Integrated Operations
Alarm Status hidden by Integrated Operations
Alarm Baseline hidden by Integrated Operations
```

Mobile uses larger product-local gaps and horizontal edge inset.

This rule is CURRENT for Integrated Operations only.

It is not yet established as a generic ADA operational UI contract.

### Tablet `1280-1365`

```text
one scope at a time
default/new state resolves to Mine
Mine visible   → only PLANTA control
Plant visible  → only MINA control
```

### Desktop `1366-2559`

```text
overview available
both MINA and PLANTA controls in overview
focused scope occupies full dashboard
opposite scope control remains while focused
close control returns to overview
```

### Videowall `>=2560`

```text
overview forced
Mine + Plant visible
presentation controls hidden
```

## Alarm Baseline ownership CURRENT

Ownership remains:

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
    └── Integrated Operations → DashboardContext / product presentation
```

Integrated Operations baseline structural contract remains:

```text
all components → main_points
bottom_point   → None
```

## Focus-aware baseline presentation CURRENT

Integrated Operations JS coordinates presentation of the existing static baseline.

While Mine or Plant is focused in tablet/desktop:

```text
filter baseline points by data-ada-scope
hide points outside visible scope
redistribute visible points over full baseline width
```

Returning to overview restores original point positions.

This does not alter AlarmBaselineProjection or Tool structure.

It is presentation-only behavior.

## Baseline controls CURRENT

The original Dashboard MINA/PLANTA controls are mounted visually inside the existing Alarm Baseline surface.

Overview:

```text
MINA at left edge
PLANTA at right edge
```

Focused:

```text
current-scope control hidden
opposite-scope control remains
```

The close control remains on the Dashboard.

Current CSS also provides:

```text
button border treatment
hover treatment
pointer cursor
baseline vertical adjustment
```

The exact final `eec22faa...` visual polish has not yet been requalified in browser after commit.

## Technical coupling note CURRENT

Integrated Operations presentation currently reaches the Generic static baseline through DOM selectors and metadata:

```text
.ada-alarm-baseline-surface
.ada-alarm-baseline-surface__point
data-ada-scope
--ada-alarm-baseline-point-x
```

This is CURRENT implementation behavior.

It must not be interpreted as a generic reusable public API until explicitly promoted by a future contract decision.

## Header CURRENT

ADA Operational Shell remains owner of the header.

Integrated Operations does not create a second header.

## KPI boundary CURRENT

Generic remains owner of KPI Collector attachment.

Current Dashboard cards contain presentation slots only.

No KPI values are connected in this closure.

Required future direction:

```text
KPI Delivery
    ↓
Generic KPI Collector
    ↓
component/store state
    ↓
Integrated Operations feature/card presentation
```

Integrated Operations must not connect directly to Cosmos.

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

Not implemented here:

```text
live alarm route overlay
origin/affected dynamic markers
alarm cards
selection
preview
rotation
dynamic runtime tone/state
```

Focus filtering of the static baseline is not the dynamic alarm overlay.

## Testing boundary

Current focused tests verify:

```text
9 component declarations
22 unique cards
Mine/Plant scope metadata
presentation targets
Tool identity projection when explicit mappings exist
shared owner/linked identity projection
JS asset packaging
```

Do not add tests whose only purpose is to freeze visual spacing, border sizes, responsive pixel geometry or branding.

Responsive/spacing/branding remain primarily visual qualification.

## Qualification state

VERIFIED:

```text
current implementation exists on atlanticus:main
complete static layout exists
9 component / 22 card contract exists
browser layout/focus path was exercised during hito
static baseline/browser coordination was exercised before final CSS-only polish
```

UNVERIFIED:

```text
exact post-eec22faa browser visual recheck
exact final-head package test rerun
exact final-head ruff format clean proof
real Tool component/subcomponent ID mappings
reusability comparison with scopes/ada/web/ui card capability
```

## Implementation state

CURRENT:

```text
Generic AdaApplicationExtension
Generic AdaApplicationDescriptor
Integrated Operations specialized application
Dashboard application module
single /
DashboardContext
complete initial 9-component / 22-card static layout
product-local bindings.py
product-local card.py
responsive presentation
Mine/Plant focus
focus-aware static baseline presentation
mobile vertical mode
tablet scope mode
videowall overview mode
```

CLOSED for this hito:

```text
complete initial Integrated Operations visual skeleton
equal-width operational overview
Mine/Plant focus interaction
initial responsive behavior
initial static Alarm Baseline presentation coordination
initial visual card inventory
```

OPEN / PLANNED:

```text
final exact visual recheck of current CSS polish
Tool ID mapping
card reuse/migration decision
first KPI-driven card/data vertical
durable restart proof
Tool hot reprojection refresh
dynamic alarm overlay
production Azure/Entra qualification
Python 3.14.7/Trixie migration
```

## Next single focus

```text
INTEGRATED-OPERATIONS-CARD-IDENTITY-AND-REUSE-BOUNDARY
```

The next chat should not start by wiring KPI data.

First determine:

```text
1. what reusable card/component presentation already exists under scopes/ada/web/ui
2. whether the current product-local card.py should reuse, migrate to, or remain separate from that capability
3. the exact authoritative Tool component/subcomponent IDs the developer must provide in bindings.py
4. the final identity contract from Tool → binding → DOM/card
```

Only after that boundary is frozen should the KPI vertical resume.

Do not mix dynamic alarms, Generic/Manager redesign, resource provisioning or final distribution into that increment.
