# ADA Generic — Configuration to Runtime

Estado: **CURRENT PHYSICAL CONTRACT / SPECIALIZED APPLICATION HANDOFF CURRENT**

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

## Generic READY derivation

```text
ToolConfiguration
→ validate ADA operational Tool
→ ToolStructure
→ bind_operational_render(
       structure,
       bottom_component_key=render_topology.bottom_component_key,
   )
→ OperationalRenderBinding
→ project_alarm_baseline(...)
→ create_application_definition(...)
```

## Specialized application derivation CURRENT

When `extension_factory` is supplied:

```text
resolve Tool Projection
    ↓
resolve OperationalRenderBinding
    ↓
extension_factory(binding)
    ↓
create Generic definition with same binding
    ↓
extend_ada_application_definition(...)
    ↓
Manager / identity / Navigation / KPI Collector / Master Projection wiring
    ↓
create Web runtime
```

A specialized application may also provide `AdaApplicationDescriptor`.

The descriptor changes product identity, not Generic runtime ownership.

## Integrated Operations CURRENT

`ada-integrated-operations-application` uses:

```text
application_descriptor =
    INTEGRATED_OPERATIONS_APPLICATION_DESCRIPTOR

extension_factory =
    create_integrated_operations_extension
```

The extension rejects a non-null binding whose Tool kind is not `INTEGRATED_OPERATIONS`.

`binding=None` remains valid so the application can start before a Tool is configured.

## No Tool

```text
UNCONFIGURED
→ OperationalRenderBinding = None
→ no static baseline
→ specialized base surface may still be present
```

No default Tool is fabricated.

## Static alarm baseline boundary

The projection/render capability remains owned by:

```text
ada.web.alarms.baseline_projection
ada.web.alarms.baseline_surface
```

Generic composes it from the same binding used by the specialized application.

Dynamic alarm runtime remains separate.

## Hot refresh gap

The worker resolves Tool Projection during bootstrap.

A Tool reprojection after worker start does not yet guarantee hot refresh of:

```text
header/branding
render topology
static baseline
specialized operational body
```

Status:

```text
PLANNED / OPEN
```

Restart remains the deterministic qualification path after Tool reprojection.

## Resource preparation

Durable Web startup consumes existing resources.

Provisioning is explicit and separate from Web startup.

Generic owns the preparation implementation through:

```text
ada.web.application.generic.manager_deployment:manager_resources_main
```

Specialized products may expose a product-named command that delegates to this implementation.

## Time Status

PI/Dispatch contracts remain separate.

No new Time Status data-feed behavior was introduced by this hito.
