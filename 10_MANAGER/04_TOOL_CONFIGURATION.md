# Manager — Tool Configuration

Estado: **FROZEN/CURRENT — TOOL STRUCTURE + RENDER TOPOLOGY CUTOVER CLOSED**

## Ownership

Tool Configuration remains ADA-specific:

```text
scopes/ada/web/tools/configuration
```

It owns:

```text
ToolConfiguration
ToolRenderTopology
BrandingConfiguration integration
ToolSourceService / codecs
Source / Projection composition
persistence composition
editor / callbacks / presentation
```

Shared structural/source contracts remain under:

```text
scopes/ada-contracts/tools
package: ada-contracts-tools==1.0.0
namespace: ada.contracts.tools
```

They include:

```text
ToolConfigurationKind
ToolScope
ToolStructure
ToolComponent
ToolSubcomponent
ToolSubcomponentAddress
ToolSourceConsumption
ToolSourceOperationalParticipation
SourceControlPolicy
ToolDependencyEntry
ToolDependencyManifest
```

`ProcessLayoutRole` is no longer part of the CURRENT contract.

## Superseded layout-role model

The following model is SUPERSEDED:

```text
ProcessLayoutRole
component.layout_role
LEFT
CENTER
RIGHT
BOTTOM as persistent ToolStructure roles
```

Do not recreate it in Tool Configuration, Manager, render binding or consumers.

## Structural authority

Tool Configuration determines which structure exists.

Runtime data determines the state of that existing structure.

A Tool may mount its static structural UI before runtime KPI/alarm data exists.

The Web application itself can start when no Tool has been published.

No default Tool is synthesized.

## ToolStructure CURRENT

### PROCESS

Required:

```text
operational_scope
center_component_key
one or more ordered components
subcomponents for every component
```

`center_component_key` must reference an existing component.

It represents semantic operational centrality, not physical center placement.

Component order is authoritative.

Process subcomponents do not declare cross-component links.

A component may omit scope and inherit the Process `operational_scope`. An explicitly equal scope is canonicalized away.

### INTEGRATED_OPERATIONS

Required:

```text
no global operational_scope
no center_component_key
scope on every component
Mine and Plant represented
Mine components before Plant components
```

The sequence in `ToolStructure.components` is authoritative.

## ToolRenderTopology CURRENT

Presentation topology is intentionally outside `ToolStructure`.

```text
ToolRenderTopology(
    bottom_component_key: str | None = None
)
```

Rules:

```text
bottom optional
bottom PROCESS only
bottom references an existing Tool component
bottom != center_component_key
```

When no bottom exists, `ToolConfiguration.to_document()` omits `render_topology`.

This keeps previous Tool documents compatible without inventing layout roles.

## Source editor behavior CURRENT

For an edit that keeps the same Tool kind:

```text
preserve structure
preserve render_topology
```

When Tool kind changes:

```text
clear structure
clear render_topology
```

The structure editor may carry render-topology editor state while editing, but persisted ownership remains `ToolConfiguration.render_topology`, not `ToolStructure`.

## Structure editor CURRENT

PROCESS exposes an optional bottom component selector.

Behavior:

```text
clearable
options are current component keys
center component excluded
Integrated Operations hides/disables bottom selection
```

The complete `ToolConfiguration` is validated before structure state is accepted.

## Alarm structural identities

`ToolStructure.alarm_baseline_component_keys` returns all components for supported operational Tool kinds.

Static alarm baseline is not the same as alarm runtime target configuration.

Alarm runtime may later target component/subcomponent identities according to its own contract.

Do not infer alarm targeting from `bottom`.

## Source CURRENT

Tool Configuration publishes:

```text
ToolSourceService
SourceStore
SourceSnapshot
SourceReleaseRef
PublishRequest
PublishResult
ConcurrencyToken
HistoryPage
```

Resource:

```text
tools/configuration.json.gz
```

## Projection CURRENT

```text
ToolProjectionBuilder
ProjectionTarget
ProjectionStore[ToolConfiguration]
SourceProjectionService[ToolConfiguration]
```

Durable projection stores serialize `ToolConfiguration.to_document()` / `from_document()`.

The optional render topology therefore follows the existing document codec boundary; no parallel persistence path exists.

## Resolution CURRENT

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

Runtime can consume an active projection without requiring Source to remain available.

## Operational Render handoff

Tool Configuration does not render the operational body directly.

```text
ToolConfiguration
    ├── structure
    └── render_topology
          ↓
OperationalRenderBinding
```

The binding preserves exact component order and derives:

```text
main_components
bottom_component
```

## Qualification observed

Render-topology closure:

```text
ada-web-tools-configuration             90 passed
render_topology + source_release gate   11 passed
ada-web-operational-render-binding      11 passed
ada-configuration-manager               64 passed
ada-generic-application                205 passed
legacy layout-role grep                PASS
git diff --check                        PASS
```

Static baseline closure after integration:

```text
alarm-baseline-projection               19 passed
alarm-baseline-surface                   9 passed
operational-render-binding              11 passed
ada-generic-application                206 passed
legacy layout-role grep                PASS
git diff --check                        PASS
```

These are local qualification results reported for the implemented increment, not a claim of full monorepo CI.

## Legacy physical retirement

`scopes/ada/web/tools/core` remains SUPERSEDED as the accepted Web contract owner and may still be BLOCKED for physical retirement by references in other scopes.

Do not create new consumers against it.
