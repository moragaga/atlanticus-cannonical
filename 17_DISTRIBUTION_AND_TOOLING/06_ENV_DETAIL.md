# env.detail Contract

Estado: **CURRENT — PHYSICAL CONTRACT CLOSED**

## Purpose

`.env.detail` is the human-readable deployment configuration contract.

It contains no real secrets.

## ADA CURRENT

```text
ATLANTICUS_ENVIRONMENT
ADA_PERSISTENCE_MODE
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE
```

Physical durable settings:

```text
ADA_STORAGE_CONTAINER_NAME
ADA_STORAGE_CONNECTION_STRING

or

ADA_STORAGE_ACCOUNT_URL
ADA_STORAGE_SAS_TOKEN

ADA_COSMOS_ENDPOINT
ADA_COSMOS_KEY
ADA_COSMOS_DATABASE_NAME
```

Defaults:

```text
ADA_APPLICATION_NAMESPACE=conciencia_situacional
ADA_STORAGE_CONTAINER_NAME=dataproduct
```

## Namespace semantics

`ADA_APPLICATION_NAMESPACE` is the application-global scope.

`ADA_TOOL_NAMESPACE` is the current Tool scope.

The next product cutover changes which domains use `application_prefix` versus `scope_prefix`; it does **not** add new physical connection variables.

No new env variable is required for Tool User Membership.

## Cosmos

Current infrastructure assumption:

```text
one Cosmos runtime/database boundary per Tool
```

Do not add multi-tool routing variables now.

## Compose-local operational ports

Operational overrides such as:

```text
ADA_COSMOS_EXPLORER_PORT
ADA_COSMOS_PORT
ADA_COSMOS_READY_PORT
ADA_AZURITE_PORT
```

are Compose-local controls, not application configuration contract fields.

## KPI consumption

Remains separate:

```text
COSMOS_CONSUMPTION_ENDPOINT
COSMOS_CONSUMPTION_KEY
COSMOS_CONSUMPTION_DATABASE_NAME
```

## Superseded

Do not restore:

```text
ADA_MANAGER_PERSISTENCE_PROVIDER
ADA_TOOL_SOURCE_PROVIDER
ADA_TOOL_PROJECTION_PROVIDER
ADA_TOOL_SOURCE_BLOB_*
ADA_TOOL_PROJECTION_COSMOS_*
```

## Runtime status

The ADA durable local contract has been exercised successfully with Azurite + Cosmos Emulator in the isolated consumer repository.
