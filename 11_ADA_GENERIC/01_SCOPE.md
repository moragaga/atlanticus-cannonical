# ADA Generic — Scope

Estado: **CURRENT — GENERIC DATA DELIVERY + STATIC STRUCTURAL PRESENTATION**

## Propósito

ADA Generic composes the reusable base needed to execute an ADA over configured contracts and expose operational state to Web consumers.

It must not absorb Tool-specific business behavior or Alarm Engine logic.

## Ownership boundary

Atlanticus provides generic infrastructure/capabilities.

ADA owns ADA-specific capabilities under `scopes/ada` and may compose them directly.

The generic Atlanticus core never depends on ADA.

## CURRENT flow

```text
environment / .env
→ provider settings
→ Tool Projection durable
→ Tool resolution
→ ToolStructure + ToolRenderTopology
→ OperationalRenderBinding
→ AlarmBaselineProjection
→ static AlarmBaselineSurface
→ KPI Collector when configured
→ Latest / Timeseries
→ process cache
→ browser/store delivery
```

## Tool resolution states

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

### READY

The Tool projection provides the static structure used to derive render binding and alarm baseline.

### UNCONFIGURED

ADA Generic still starts.

It creates no default Tool, no render binding and no alarm baseline.

### UNAVAILABLE / INVALID

The existing degraded-resolution path remains in force. The static baseline is not fabricated from incomplete state.

## Generic presentation boundary

ADA Generic now owns one structural presentation that is common enough to be generic:

```text
AlarmBaselineSurface
```

It represents configured Tool structure only.

It does not represent live alarm state.

Specific operational bodies remain outside the generic contract where Tool-specific visualization is required.

## Operational Render Binding

CURRENT binding contains:

```text
ToolStructure
ordered OperationalComponentBinding values
bottom_component_key | None
```

Derived:

```text
main_components
bottom_component
```

It contains no KPI state and no live alarm state.

## Static Alarm Baseline

CURRENT projection:

```text
AlarmBaselineProjection
    main_points
    bottom_point | None
```

Current anchors are component identities.

The surface renders no visible component labels.

Identity is retained as metadata for future dynamic overlay.

## Dynamic alarm boundary

Not part of this closure:

```text
alarm-live-projection read
routes
origin/affected
cards
selection
preview
severity state
management state
```

Those must arrive from the authoritative alarm runtime/modeler contracts.

## Rule

```text
CONFIGURATION
determines structure

RUNTIME DATA
determines live state

GENERIC APPLICATION
composes stable generic boundaries

CONCRETE TOOL
owns Tool-specific body/presentation
```

Static baseline is structure, not live state.
