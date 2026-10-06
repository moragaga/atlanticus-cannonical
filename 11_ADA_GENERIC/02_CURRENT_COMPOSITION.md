# ADA Generic — Current Composition

Estado: **CURRENT — ADDITIVE SPECIALIZED APPLICATION CONTRACT + TOOL RESOLUTION + STATIC ALARM BASELINE**

## Generic ownership CURRENT

ADA Generic owns reusable composition/runtime for:

```text
settings
local/durable Manager composition
identity binding
Tool Projection resolution
OperationalRenderBinding
static AlarmBaselineProjection
static AlarmBaselineSurface module/layout
ADA Master Projection composition
KPI Collector attachment
Web runtime lifecycle
resource preparation implementation
```

A specialized ADA application must reuse these capabilities rather than copy them.

## Specialized application identity CURRENT

`AdaApplicationDescriptor` allows a specialized product to retain Generic runtime while declaring its own:

```text
import_name
application_id
display_name
distribution_name
application_root
```

Generic uses `GENERIC_APPLICATION_DESCRIPTOR` by default.

A specialized descriptor changes application identity and publications root without replacing Generic runtime contracts.

## Additive extension CURRENT

`AdaApplicationExtension` is the specialized application extension boundary.

Contract:

```text
modules: tuple[WebModule, ...]
page_packages: tuple[str, ...] | None
```

Application definition extension:

```text
Generic definition
    +
extension.modules
    ↓
final modules

extension.page_packages is None
    → retain Generic page packages

extension.page_packages is tuple
    → use that application-level page-package selection
```

Specialized products do not replace Generic bootstrap.

## Tool composition CURRENT

READY Tool with specialized extension:

```text
ToolConfiguration
    ├── display_name
    ├── branding
    ├── structure
    └── render_topology
           ↓
resolve OperationalRenderBinding once
           ├── specialized extension factory
           └── Generic definition/static baseline
                     ↓
              extended Web definition
```

The same binding is supplied to the specialized extension and Generic Tool definition.

This prevents a specialized body and Generic static baseline from deriving independent structural interpretations.

## No Tool CURRENT

```text
UNCONFIGURED
→ OperationalRenderBinding = None
→ specialized extension may still compose its empty/base surface
→ no AlarmBaselineProjection
```

The application remains valid.

No implicit/default Tool is created.

## Alarm surface modules CURRENT

```text
ada-alarm-baseline-surface
ada-alarm-management-summary
ada-alarm-status
```

The baseline capability lives under `ada.web.alarms`.

Generic composes/injects it; Generic does not own its alarm-domain implementation.

## Asset ordering CURRENT

Relevant Generic layers:

```text
ada_alarm_management_summary  130
ada_alarm_status              140
ada_alarm_baseline_surface    145
ada_time_status               150
```

The baseline remains at `145`.

## Namespace

```text
application_namespace = ADA_APPLICATION_NAMESPACE
scope_namespace       = ADA_TOOL_NAMESPACE
```

## Source ownership CURRENT

Tool-scoped Source covers Tool-specific domains.

Global Users identity registry remains application-scoped.

## Session CURRENT

```text
Identity
→ users-runtime resolve
→ RuntimeUser
→ UsersRuntime
→ Manager/Navigation principal
```

## Current specialized consumer

`ada-integrated-operations-application` is a CURRENT consumer of this extension contract.

It provides:

```text
INTEGRATED_OPERATIONS_APPLICATION_DESCRIPTOR
create_integrated_operations_extension(binding)
```

and keeps Generic as the runtime owner.

## Distribution

This closure verifies implementation on `atlanticus:main` and browser/runtime behavior reported for Integrated Operations.

It does not qualify a new final distributed artifact.

Distribution/tooling remains a separate track.
