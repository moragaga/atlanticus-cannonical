# ADA Generic — Current Composition

Estado: **CURRENT — TOOL RESOLUTION + STATIC ALARM BASELINE COMPOSED**

## Composition root CURRENT

ADA Generic owns product composition for:

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
```

## Tool composition CURRENT

READY Tool:

```text
ToolConfiguration
    ├── display_name
    ├── branding
    ├── structure
    └── render_topology
           ↓
OperationalRenderBinding
           ↓
AlarmBaselineProjection
           ↓
create_application_definition(...)
           ↓
operational layout
```

The baseline is mounted below the operational header and before main application content when a projection exists.

## No Tool CURRENT

```text
UNCONFIGURED
→ base application definition
→ no OperationalRenderBinding
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

The baseline module is static and neutral; the other alarm surfaces retain their own contracts.

## Asset ordering CURRENT

Relevant layers:

```text
ada_alarm_management_summary  130
ada_alarm_status              140
ada_alarm_baseline_surface    145
ada_time_status               150
```

The baseline moved from the historical `150` collision to `145`.

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

## Distribution

This hito qualified source/runtime tests only.

It did not create or qualify a new final distributed artifact.

Distribution/tooling remains a separate track.
