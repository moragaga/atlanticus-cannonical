# ADA Web — Source Ledger

Estado: **AUDIT LEDGER / INTEGRATED OPERATIONS FOUNDATION 2026-10-05**

## Implementation CURRENT audited

```text
moragaga/atlanticus@6ecbfb21dd0f98d7cae8f0c142d796994a7fc361
```

Commit date:

```text
2026-10-05T21:45:17Z
```

## Decisions

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

No active historical decision was verified that supersedes the CURRENT implementation described here.

## Canonical base before replacement

```text
moragaga/atlanticus-cannonical@ed26b441b3055e582ba272e349f8cd6de0e067fa
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

## Static baseline checkpoint retained

```text
AlarmBaselineProjection.main_points
AlarmBaselineProjection.bottom_point
AlarmBaselineSurface static traces
ADA Generic integration
asset order 145
```

## Additive application checkpoint

```text
atlanticus@a334d14b1e0481a04676492c2daa3bf8aa436380
```

Introduced:

```text
AdaApplicationExtension
extension_factory in Generic host/bootstrap
same OperationalRenderBinding shared with extension and Generic definition
```

## Integrated Operations foundation checkpoint

```text
atlanticus@2ddb968c79d203a5e6bdc2acf6ec6da1e36dc9c6
```

Introduced:

```text
AdaApplicationDescriptor
ada-integrated-operations-application
Dashboard as the single product application module
single route /
Mine and Plant as internal Dashboard composition
DashboardContext service
separate Dashboard/Mine/Plant asset layers
```

## Local operation checkpoint

```text
atlanticus@6ecbfb21dd0f98d7cae8f0c142d796994a7fc361
```

Introduced:

```text
deployment/local/compose.yaml
ada-integrated-operations-resources
durable local .env.detail guidance
```

## Browser/runtime evidence reported

```text
Integrated Operations app running             VERIFIED
projected real Tool consumed                   VERIFIED
Mina / Planta rendered                         VERIFIED
static Alarm Baseline rendered                 VERIFIED
component identity/name metadata observed      VERIFIED
```

Not reported:

```text
clean restart after projection without Manager changes
specialized resources prepare/validate command output
```

Classification:

```text
UNVERIFIED / OPEN
```

## Reference source

```text
moragaga/isolated-web-functions:main
operational_trace/
assets/operational_trace/
```

Classification:

```text
REFERENCE
```

Used for static baseline visual geometry and future interaction knowledge only.

## Canonical drift reconciled by this replacement

Previous canonical stated:

```text
Integrated Operations full Tool body PLANNED
browser visual qualification OPEN
Generic specialized application extension not documented
```

Current implementation/evidence states:

```text
Integrated Operations foundation CURRENT
Dashboard/Mine/Plant base composition CURRENT
Integrated Operations baseline browser observation VERIFIED
Generic additive extension CURRENT
specialized application descriptor CURRENT
```

## Open

```text
durable restart proof
Tool hot reprojection refresh
KPI-driven operational modules
dynamic alarm overlay
subcomponent runtime anchors
Process visual variants
responsive/videowall qualification
```
