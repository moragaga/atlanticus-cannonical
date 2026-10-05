# ADA Web — Source Ledger

Estado: **AUDIT LEDGER / STATIC ALARM BASELINE 2026-10-05**

## Implementation CURRENT audited

```text
moragaga/atlanticus@686a80f6a05eeea93d35d642cf2f92100cb1e61b
```

Commit date:

```text
2026-10-05T19:12:27Z
```

## Decisions

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

No specific active decision contradiction was textually verified during this closure.

Historical decision documents containing Tool/Structure evolution exist, but exact binary DOCX wording was not used to invent additional constraints.

## Canonical base before replacement

```text
moragaga/atlanticus-cannonical@44d3c803f60d1a1630d3a3374a663447cfe21248
```

## Relevant implementation scopes

```text
scopes/ada-contracts/tools/
scopes/ada/web/tools/configuration/
scopes/ada/web/operational-render-binding/
scopes/ada/web/alarms/baseline-projection/
scopes/ada/web/alarms/baseline-surface/
scopes/ada/web/application/ada-generic-application/
```

## Render topology checkpoint

```text
ToolRenderTopology.bottom_component_key
OperationalRenderBinding.main_components
OperationalRenderBinding.bottom_component
```

Qualification:

```text
tools/configuration                 90 passed
render-topology/source-release      11 passed
operational-render-binding          11 passed
configuration-manager               64 passed
generic application                205 passed
```

## Static baseline checkpoint

```text
AlarmBaselineProjection.main_points
AlarmBaselineProjection.bottom_point
AlarmBaselineSurface static traces
ADA Generic integration
asset order 145
```

Qualification:

```text
baseline-projection                 19 passed
baseline-surface                     9 passed
operational-render-binding          11 passed
generic application                206 passed
legacy layout-role grep            PASS
git diff --check                    PASS
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

Used for visual geometry and future interaction knowledge only.

## Canonical conflicts found before replacement

Stale canonical text declared:

```text
ProcessLayoutRole
component layout_role
PROCESS center-only alarm baseline
OperationalRenderBinding as structure-only
UI / Alarm integration PLANNED
```

Current implementation declares:

```text
no layout roles
center_component_key semantic center
optional ToolRenderTopology.bottom_component_key
binding main/bottom topology
PROCESS all-component static baseline
Generic static baseline integration CURRENT
```

Classification:

```text
IMPLEMENTATION CURRENT
CANONICAL STALE
```

These replacement files reconcile that canonical drift.

## Open

```text
visual browser qualification
dynamic alarm overlay
subcomponent runtime anchors
full Operational Trace behaviors
hot Tool reprojection refresh
```
