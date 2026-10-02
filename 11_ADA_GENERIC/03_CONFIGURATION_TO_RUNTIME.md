# ADA Generic — Configuration to Runtime

Estado: **CURRENT — ENVIRONMENT/PERSISTENCE CONTRACT FROZEN FOR CURRENT RUNTIME**

## Primary settings

```text
ATLANTICUS_ENVIRONMENT
ADA_PERSISTENCE_MODE
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE
ADA_TOOL_LOCAL_BASE_ROOT
ADA_TOOL_SOURCE_BLOB_CONTAINER_NAME
ADA_TOOL_SOURCE_BLOB_CONNECTION_STRING
ADA_TOOL_SOURCE_BLOB_ACCOUNT_URL
ADA_TOOL_SOURCE_BLOB_SAS_TOKEN
ADA_TOOL_PROJECTION_COSMOS_ENDPOINT
ADA_TOOL_PROJECTION_COSMOS_KEY
ADA_TOOL_PROJECTION_COSMOS_DATABASE_NAME
COSMOS_CONSUMPTION_ENDPOINT
COSMOS_CONSUMPTION_KEY
COSMOS_CONSUMPTION_DATABASE_NAME
```

## Axes

```text
ATLANTICUS_ENVIRONMENT
→ host/runtime behavior

ADA_PERSISTENCE_MODE
→ local | durable persistence
```

Emulator/Azure are connection targets, not extra architecture modes.

## Storage credentials

Supported:

```text
connection string
OR
account URL + SAS token
```

Do not configure both simultaneously.

## Master Projection identity

No manual path variable.

Relative identity:

```text
master-projection/material.zip
```

Provision command:

```text
uv run ada-generic-master-projection generate --user <service-user>
```

Local and Blob paths are derived from application namespace.

## Source namespace

Current ADA-specific helper:

```text
AdaStorageNamespace(
    application_namespace,
    tool_namespace,
)
```

NEXT shared front must inspect whether this is actually a generic namespace capability and remove
cross-product dependency without changing `SourceStore`.

## KPI Delivery

Optional and separate from Tool Projection Cosmos.

Not part of the next Source increment.
