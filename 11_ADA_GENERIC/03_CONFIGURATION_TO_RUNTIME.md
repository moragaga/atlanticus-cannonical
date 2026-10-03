# ADA Generic — Configuration to Runtime

Estado: **CURRENT PHYSICAL CONTRACT / TOOL-SCOPE OWNERSHIP CUTOVER PLANNED**

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

Superseded Tool-specific physical Storage/Cosmos names must not return.

## Namespace

```text
StorageNamespace(
    application_namespace=ADA_APPLICATION_NAMESPACE,
    scope_namespace=ADA_TOOL_NAMESPACE,
)
```

For the current real ADA example:

```text
conciencia_situacional/operaciones_integradas
```

is the Tool prefix.

## Durable ownership target

Application-global:

```text
users identity registry
```

Tool-scoped:

```text
tools
profiles
navigation
ada-access
operational
tool-user-membership
kpis
kpi-definitions
tool users recovery snapshot
```

Implementation still needs the cutover for Navigation/Profiles/Access/Operational and Users.

## Runtime users target

Login/runtime reads one complete Tool-specific `users-runtime` document rather than joining Blob contracts.

The snapshot carries:

```text
identity
enabled
resolved profile
resolved operational
```

Operational keys are always present and nullable.

Access is resolved separately from the profile key.

## Tool runtime refresh gap

Tool `display_name` already flows into `OperationalBrandState.context_name`, but the current worker resolves Tool Projection during bootstrap.

A Tool reprojection after worker start does not yet guarantee hot refresh of header/branding/runtime context.

Status:

```text
PLANNED / LATER
```

## Time Status gap

PI/Dispatch labels and freshness contracts already exist.

The runtime source/timestamp pipeline needed to feed Time Status remains `PLANNED`.

## KPI Registry / Delivery target

KPI Registry projection/materialization will include `tool_key` derived from Tool Projection for Delivery.

Do not store an independent editable Tool name in KPI Source.
