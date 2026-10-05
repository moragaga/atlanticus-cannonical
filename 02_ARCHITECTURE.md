# Atlanticus — Architecture

Estado: **CURRENT — MODULAR PRODUCT COMPOSITION + EXPLICIT TOOL RENDER TOPOLOGY**

## Regla principal

Atlanticus es modular y reusable. ADA y Command Center son consumidores.

ADA puede depender de Atlanticus. El núcleo genérico de Atlanticus no depende de ADA.

## Generic Web capabilities

```text
Source Core / Local / Blob
Projection Core
Storage Namespace
Storage Topology
Users
Profiles
Navigation
Manager
Master Projection
```

## Storage Namespace CURRENT

```text
StorageNamespace(application_namespace, scope_namespace)

application_prefix = <application_namespace>
scope_prefix       = <application_namespace>/<scope_namespace>
```

## ADA ownership CURRENT

### Application-global

```text
Users identity registry
```

### Tool-scoped

```text
Tool Configuration
Profiles
Navigation
ADA Access
Operational
Tool User Membership
KPI Registry
KPI Definitions
Tool Users Recovery artifacts
```

## Tool structural authority CURRENT

Structural contracts shared by ADA products live under:

```text
scopes/ada-contracts/tools
namespace: ada.contracts.tools
```

`ToolStructure` is domain structure.

It owns:

```text
tool_key
kind
ordered components
operational_scope when applicable
center_component_key for PROCESS
component/subcomponent identity and linking
```

It does **not** own presentation roles such as LEFT/CENTER/RIGHT/BOTTOM.

`ProcessLayoutRole`, `layout_role` and equivalent persistent layout-role semantics are SUPERSEDED.

### PROCESS

```text
operational_scope       required
center_component_key    required
components              ordered authoritative sequence
```

`center_component_key` means semantic operational centrality. It does not mean physical 50% placement.

### INTEGRATED_OPERATIONS

```text
operational_scope       absent
center_component_key    absent
component scope         required
Mine + Plant            required
component order         Mine → Plant
```

## Render topology CURRENT

Web presentation topology belongs to Tool Configuration, outside `ToolStructure`.

```text
ToolRenderTopology(
    bottom_component_key: str | None
)
```

Invariant:

```text
bottom only for PROCESS
bottom references an existing component
bottom != center
bottom optional
```

If empty, `render_topology` is omitted from the serialized Tool Configuration document.

## Operational render binding CURRENT

```text
ToolStructure
    + optional bottom_component_key
        ↓
OperationalRenderBinding
```

Binding contains one component binding per Tool component in exact `ToolStructure.components` order.

Derived views:

```text
component_keys
main_components
main_component_keys
bottom_component
```

`main_components` means all structure components except the optional bottom component.

The binding contains no KPI runtime state and no alarm runtime state.

## Static alarm baseline CURRENT

The static baseline is a Web projection of already-resolved structure/topology:

```text
ToolStructure + bottom
        ↓
AlarmBaselineProjection
    main_points
    bottom_point | None
        ↓
AlarmBaselineSurface
```

CURRENT anchor kind:

```text
COMPONENT
```

Subcomponent alarm anchors remain a future dynamic/runtime concern; they are not materialized by the static surface today.

The surface stores operational identity in `data-*` attributes but does not render component names visibly.

## Dynamic alarm boundary

Static baseline and dynamic alarm state are separate contracts.

Future direction:

```text
AlarmBaselineProjection      Alarm runtime/live projection
          |                           |
          +------------+--------------+
                       ↓
              dynamic alarm surface
```

The Web layer must not recalculate priority, lifecycle, routing or management rules owned by Alarm Engine/Modeler.

## Empty Tool behavior CURRENT

ADA Generic can start without a configured Tool.

```text
ToolProjectionResolution.UNCONFIGURED
    ↓
base application definition
    ↓
no OperationalRenderBinding
no AlarmBaselineProjection
no implicit/default Tool
```

An absent Tool is not synthesized to satisfy presentation.

## Cosmos CURRENT

```text
one Cosmos database/runtime boundary per Tool
```

## Users CURRENT

```text
Global UserIdentity
+ ToolUserMembership
+ Profiles
+ Operational
→ materialization
→ RuntimeUser
```

Global identity does not contain Tool-specific profile/enabled state.

## Runtime/session CURRENT

```text
identity provider
→ users-runtime.resolve(identity)
→ RuntimeUser
→ session / Manager principal / Navigation principal
```

## Access

ADA Access remains separate:

```text
profile_key -> access_keys
```

## Clean cutover rule

```text
contracts before consumers
clean replacement
no legacy aliases
no dual write
no compatibility storage path
```
