# ADA Web — Source Ledger

Estado: **AUDIT LEDGER / INTEGRATED OPERATIONS INITIAL UI CLOSURE 2026-10-06**

## Implementation CURRENT audited

```text
moragaga/atlanticus@eec22faa9cca5ad67af8e5fe0bf0299264e27ad5
```

Commit date:

```text
2026-10-06T17:00:49Z
```

## Decisions

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

No active historical decision was verified that supersedes the CURRENT implementation described here.

## Canonical base before replacement

```text
moragaga/atlanticus-cannonical@c1c5479930ebbde45d3f4b636ba6f1d72f7db13b
```

## Relevant implementation scopes

```text
scopes/ada-contracts/tools/
scopes/ada/web/tools/configuration/
scopes/ada/web/operational-render-binding/
scopes/ada/web/alarms/baseline-projection/
scopes/ada/web/alarms/baseline-surface/
scopes/ada/web/application/ada-generic-application/
scopes/ada/web/application/ada-integrated-operations-application/
```

## Previous checkpoints retained

```text
atlanticus@a334d14b1e0481a04676492c2daa3bf8aa436380
    AdaApplicationExtension
    same OperationalRenderBinding shared by Generic and specialized extension

atlanticus@2ddb968c79d203a5e6bdc2acf6ec6da1e36dc9c6
    AdaApplicationDescriptor
    Integrated Operations specialized application
    Dashboard as single product application module
    single route /
    DashboardContext
    Mine/Plant internal composition

atlanticus@6ecbfb21dd0f98d7cae8f0c142d796994a7fc361
    local Azurite/Cosmos harness
    product-named resource command delegating to Generic preparation
```

## Complete initial Dashboard checkpoint

```text
atlanticus@ed1d1afab987cf098926e100f5bd4137d418e149
```

Introduced:

```text
DashboardCardBinding
DashboardComponentBinding
DashboardSharedCardBinding
product-local bindings.py
product-local card.py
9 operational component declarations
22 visual cards
complete Mine/Plant static layout
Dashboard presentation JS asset
focused dashboard tests
```

The 22-card inventory is structural presentation only; KPI values are not yet wired.

## Responsive/focus checkpoints

Intermediate presentation work culminated in:

```text
atlanticus@f16ce932d0bc902c84419a5e3e5d28ab9ed49c3f
```

Current behavior from this line of work:

```text
single 9-track overview grid
uniform component gaps
mobile vertical presentation
tablet single-scope presentation
desktop overview + focus
videowall fixed overview
focus-aware static Alarm Baseline point filtering/repositioning
Mine/Plant controls mounted on Alarm Baseline surface
```

## Final current CSS checkpoint

```text
atlanticus@eec22faa9cca5ad67af8e5fe0bf0299264e27ad5
```

Changed only Integrated Operations Dashboard CSS and pedagogical mirror.

Current final polish includes:

```text
dashboard edge inset and border
mobile horizontal inset
raised baseline presentation position
button border/background/shadow treatment
pointer cursor and hover styling
```

## Current implementation files inspected

```text
.../modules/dashboard/bindings.py
.../modules/dashboard/card.py
.../modules/dashboard/layout.py
.../modules/dashboard/mine/layout.py
.../modules/dashboard/plant/layout.py
.../modules/dashboard/resources/css/10_dashboard.css
.../modules/dashboard/resources/js/10_presentation.js
.../modules/dashboard/mine/resources/css/10_mine.css
.../modules/dashboard/plant/resources/css/10_plant.css
.../tests/modules/dashboard/test_layout.py
.../tests/modules/dashboard/test_assets.py
```

## Browser/runtime evidence reported during hito

Verified before the final CSS-only polish:

```text
specialized application starts
full-width operational Dashboard renders
9 operational columns render
22 cards render
Mine/Plant focus changes visible scope
static baseline renders
focus updates baseline scope presentation
responsive layouts were manually iterated
```

Current final `eec22faa...` browser recheck:

```text
UNVERIFIED
```

The final commit is CSS-only relative to the preceding behavior checkpoint, but no exact post-commit screenshot/run was captured before closure.

## Automated evidence reported during hito

At an earlier responsive checkpoint:

```text
uv run --with pytest==9.1.1 pytest -q
→ 5 passed
```

Current package test files still contain five focused tests.

No exact test rerun against `eec22faa...` was captured.

Classification:

```text
earlier focused suite     VERIFIED
final-head focused suite  UNVERIFIED
```

Earlier style evidence also showed:

```text
ruff check          PASS
ruff format --check pending formatting on 6 Python files
```

No later final-head clean formatting proof was captured.

Classification:

```text
ruff final-head qualification UNVERIFIED / OPEN
```

## Reference sources used in design

```text
moragaga/isolated-web-functions:main
    alarm/component visual correlation reference

moragaga/__temporal_ada_latest:main
    currently deployed/historical ADA layout reference

moragaga/atlanticus-multi-stage:main
    historical 4+5 Integrated Operations geometry and focus reference
```

Classification:

```text
REFERENCE ONLY
```

None of these repositories transfer authority, contracts or ownership to Atlanticus.

## Superseded during this hito

```text
Integrated Operations Dashboard as empty/base Mine/Plant containers only
first KPI vertical as immediate next implementation before layout completion
50/50 Mine/Plant base layout
focus as merely PLANNED
225% / 180% width expansion plus translate focus technique
mobile horizontal component carousel
tablet showing both same-scope and opposite-scope controls
responsive/videowall entirely OPEN
```

## Refined CURRENT decisions

```text
Integrated Operations presentation is product-owned
Generic remains runtime owner
Alarm Baseline remains Generic/alarm-owned
Integrated Operations may coordinate static baseline presentation by existing DOM scope metadata
9 equal overview tracks are the visual alignment contract
Mine = 4 tracks
Plant = 5 tracks
shared Carguío/Transporte card is not a tenth Tool component
visual keys are separate from authoritative Tool keys
```

## Canonical drift reconciled by this replacement

Previous canonical stated:

```text
Mine/Plant only provide base content containers
final operational component/card content not implemented
focus implementation not claimed
responsive/videowall qualification open
next single focus = first KPI operational data vertical
```

Current implementation states:

```text
complete initial 9-component/22-card static layout CURRENT
focus implementation CURRENT
responsive modes CURRENT
focus-aware static baseline presentation CURRENT
Tool identity mapping still OPEN
KPI content still PLANNED
```

## Open

```text
exact final-head visual requalification
exact final-head focused test rerun
ruff format clean proof
authoritative Tool component/subcomponent ID mapping in bindings.py
decision on reuse/migration of product-local card shell vs scopes/ada/web/ui
first KPI-driven operational data vertical
durable restart proof
Tool hot reprojection refresh
dynamic alarm-live overlay
final distribution qualification
Python 3.14.7 / Trixie migration
production Azure / Entra qualification
```
