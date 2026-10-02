# env.detail Contract

Estado: **CURRENT — DISTRIBUTION CONFIGURATION CONTRACT CONVERGED**

## Purpose

`.env.detail` is the human-readable configuration contract from which deployment configuration templates are derived.

It must not contain real secrets.

Each variable should make clear:

```text
purpose
required/optional
accepted values/format
secret/non-secret
manual/derived/default
owner/consumer
```

## Frozen architecture rule

```text
environment
!=
persistence mode/provider
!=
connection target
```

Local host may use durable persistence.

Azure and local emulators use the same application-level durable contract where that contract applies.

## ADA CURRENT

Core axes:

```text
ATLANTICUS_ENVIRONMENT
ADA_PERSISTENCE_MODE
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE
```

Frozen logical defaults/ownership:

```text
ADA_APPLICATION_NAMESPACE    default: conciencia_situacional
ADA_TOOL_NAMESPACE           explicit per distributed Tool/application
```

Frozen physical durable contract:

```text
ADA_STORAGE_CONTAINER_NAME
ADA_STORAGE_CONNECTION_STRING

OR

ADA_STORAGE_ACCOUNT_URL
ADA_STORAGE_SAS_TOKEN

AND

ADA_COSMOS_ENDPOINT
ADA_COSMOS_KEY
ADA_COSMOS_DATABASE_NAME
```

Default Storage container:

```text
dataproduct
```

Logical isolation remains the responsibility of:

```text
StorageNamespace(
    application_namespace,
    scope_namespace,
)
```

The physical Storage/Cosmos connection is application-level. It is not named as a Tool-specific physical connection.

### Superseded ADA variables

The following are SUPERSEDED and have no compatibility alias in the current contract:

```text
ADA_MANAGER_PERSISTENCE_PROVIDER
ADA_TOOL_SOURCE_PROVIDER
ADA_TOOL_PROJECTION_PROVIDER
ADA_TOOL_SOURCE_BLOB_CONTAINER_NAME
ADA_TOOL_SOURCE_BLOB_CONNECTION_STRING
ADA_TOOL_SOURCE_BLOB_ACCOUNT_URL
ADA_TOOL_SOURCE_BLOB_SAS_TOKEN
ADA_TOOL_PROJECTION_COSMOS_ENDPOINT
ADA_TOOL_PROJECTION_COSMOS_KEY
ADA_TOOL_PROJECTION_COSMOS_DATABASE_NAME
```

`ADA_TOOL_NAMESPACE` remains CURRENT because it is a real logical namespace, not a physical connection identifier.

### Local distributed Compose

The distributed ADA full Compose injects:

```text
ADA_PERSISTENCE_MODE=durable
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE
ADA_STORAGE_CONTAINER_NAME
ADA_STORAGE_CONNECTION_STRING
ADA_COSMOS_ENDPOINT
ADA_COSMOS_KEY
ADA_COSMOS_DATABASE_NAME
```

Its default Storage container is:

```text
dataproduct
```

`ADA_LOCAL_BLOB_CONTAINER` may override the local container for isolated emulator tests without changing the external production contract.

### KPI consumption connection

The optional KPI Delivery/Consumption connection remains a separate contract:

```text
COSMOS_CONSUMPTION_ENDPOINT
COSMOS_CONSUMPTION_KEY
COSMOS_CONSUMPTION_DATABASE_NAME
```

It was not merged into the ADA application durable connection in this hito.

## Command Center CURRENT

Core axes:

```text
ATLANTICUS_ENVIRONMENT
ADA_MANAGER_PERSISTENCE_PROVIDER
```

Command Center physical durable contract:

```text
ADA_COMMAND_CENTER_STORAGE_CONNECTION_STRING
ADA_COMMAND_CENTER_STORAGE_CONTAINER_NAME
ADA_COMMAND_CENTER_COSMOS_ENDPOINT
ADA_COMMAND_CENTER_COSMOS_DATABASE_NAME
ADA_COMMAND_CENTER_COSMOS_KEY
```

Default Storage container:

```text
dataproduct
```

External named Tool Cosmos connections remain separate and dynamic. They are not static required variables of the Command Center distribution contract.

Master material remains derived under the frozen namespace:

```text
conciencia_situacional/command-center/master-projection/material.zip
```

## Distribution templates

Configuration templates are CURRENT for:

```text
ADA
Command Center
```

They generate environment mappings for:

```text
DEV
UAT
PRD
```

Production secrets are represented as secret references/placeholders, not real secret values.

Generic Web does not generate configuration templates.

## OPEN

The contract definition is CLOSED.

The following runtime qualification is still OPEN:

```text
ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE
```

Production Azure App Settings / Key Vault / Entra runtime qualification remains separate.
