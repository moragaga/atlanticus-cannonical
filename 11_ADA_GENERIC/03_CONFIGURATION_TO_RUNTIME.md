# ADA Generic — Configuration to Runtime

Estado: **CURRENT PHYSICAL CONTRACT / TOOL STRUCTURE TO STATIC PRESENTATION CLOSED**

## Primary settings CURRENT

```text
ATLANTICUS_ENVIRONMENT
ADA_PERSISTENCE_MODE
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE

ADA_STORAGE_CONTAINER_NAME
ADA_STORAGE_CONNECTION_STRING

or

ADA_STORAGE_ACCOUNT_URL
ADA_STORAGE_SAS_TOKEN

ADA_COSMOS_ENDPOINT
ADA_COSMOS_KEY
ADA_COSMOS_DATABASE_NAME

COSMOS_CONSUMPTION_ENDPOINT
COSMOS_CONSUMPTION_KEY
COSMOS_CONSUMPTION_DATABASE_NAME
```

## Namespace

```text
StorageNamespace(
    application_namespace=ADA_APPLICATION_NAMESPACE,
    scope_namespace=ADA_TOOL_NAMESPACE,
)
```

## Tool document CURRENT

Relevant structure:

```text
ToolConfiguration
    tool_key
    display_name
    kind
    source_consumption
    source_operational_participation
    structure
    render_topology
    branding
```

`render_topology` is omitted when empty.

CURRENT topology:

```text
bottom_component_key | None
```

## Runtime structural derivation

READY projection:

```text
ToolConfiguration
→ validate ADA operational Tool
→ ToolStructure
→ bind_operational_render(
       structure,
       bottom_component_key=render_topology.bottom_component_key,
   )
→ project_alarm_baseline(...)
→ create_application_definition(
       alarm_baseline_projection=...
   )
```

If an external composition supplies a render binding, it must match both the Tool Structure and the configured bottom component.

## No Tool

```text
UNCONFIGURED
→ create_application_definition()
→ no static baseline
```

This is an explicit supported state.

## Hot refresh gap

The worker resolves Tool Projection during bootstrap.

A Tool reprojection after worker start does not yet guarantee hot refresh of:

```text
header/branding
render topology
static baseline
operational body
```

Status:

```text
PLANNED / LATER
```

## Dynamic alarms

This flow does not consume `alarm-live-projection`.

Static Tool presentation and dynamic alarm delivery remain separate boundaries.

## Time Status

PI/Dispatch contracts remain separate.

No new Time Status data-feed behavior was introduced by this hito.
