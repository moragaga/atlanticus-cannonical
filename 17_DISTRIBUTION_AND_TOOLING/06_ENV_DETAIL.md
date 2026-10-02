# env.detail Contract

Estado: **CURRENT — DUAL APP CONTRACT AUDIT CLOSED**

## Purpose

`.env.detail` is a human-readable configuration contract.

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
persistence provider
!=
connection target
```

Local host may use durable persistence.

Azure and emulators use the same application contract.

## ADA CURRENT

Key axes:

```text
ATLANTICUS_ENVIRONMENT
ADA_PERSISTENCE_MODE
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE
```

Durable:

```text
Blob container + connection string
OR account URL + SAS

Tool Projection Cosmos endpoint/key/database
```

Master material location is derived.

Provision command:

```text
uv run ada-generic-master-projection generate --user <service-user>
```

## Command Center CURRENT

Key axes:

```text
ATLANTICUS_ENVIRONMENT
ADA_MANAGER_PERSISTENCE_PROVIDER
```

Durable:

```text
ADA_COMMAND_CENTER_STORAGE_CONNECTION_STRING
ADA_COMMAND_CENTER_STORAGE_CONTAINER_NAME
ADA_COMMAND_CENTER_COSMOS_ENDPOINT
ADA_COMMAND_CENTER_COSMOS_DATABASE_NAME
ADA_COMMAND_CENTER_COSMOS_KEY
```

External named Tool Cosmos connections remain separate.

Master material is derived:

```text
conciencia_situacional/command-center/master-projection/material.zip
```

Provision command:

```text
uv run ada-command-center-master-projection generate --user <service-user>
```

## OPEN

No configuration-contract redesign is NEXT.

The next focus is Source/namespace ownership.

Production Key Vault/App Settings mapping remains separate.
