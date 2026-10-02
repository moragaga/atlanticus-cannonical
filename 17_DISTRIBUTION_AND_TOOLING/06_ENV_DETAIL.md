# env.detail Contract

Estado: **CURRENT POLICY / ADA + COMMAND CENTER AUDIT NEXT**

## Purpose

`.env.detail` is a human-readable configuration contract.

It must not contain real secrets.

Each variable should make clear:

```text
purpose
required/optional
accepted values or format
secret/non-secret
local/production applicability
manual/derived/default
owner/consumer
expected infrastructure source
```

## ADA CURRENT

Current `ada-generic-application/.env.detail` documents:

```text
ATLANTICUS_ENVIRONMENT
ADA_PERSISTENCE_MODE
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE
ADA_TOOL_SOURCE_BLOB_CONTAINER_NAME
ADA_TOOL_SOURCE_BLOB_CONNECTION_STRING
optional Blob account URL + SAS
ADA_TOOL_PROJECTION_COSMOS_ENDPOINT
ADA_TOOL_PROJECTION_COSMOS_KEY
ADA_TOOL_PROJECTION_COSMOS_DATABASE_NAME
optional KPI Delivery Cosmos variables
```

Master Projection identity is derived:

```text
master-projection/material.zip
```

No manual Master material path variable is part of the current runtime contract.

Historical:

```text
ADA_MASTER_PROJECTION_MATERIAL_PATH
```

is **SUPERSEDED**.

## Command Center CURRENT

Current `.env.detail` documents:

```text
ATLANTICUS_ENVIRONMENT
ADA_MANAGER_PERSISTENCE_PROVIDER
ATLANTICUS_LOCAL_IDENTITY_SUBJECT_ID optional
ADA_COMMAND_CENTER_STORAGE_CONNECTION_STRING
ADA_COMMAND_CENTER_STORAGE_CONTAINER_NAME
ADA_COMMAND_CENTER_COSMOS_ENDPOINT
ADA_COMMAND_CENTER_COSMOS_DATABASE_NAME
ADA_COMMAND_CENTER_COSMOS_KEY
dynamic external Tool Cosmos variables
APPLICATION_PUBLICATIONS_ROOT optional
```

Important:

```text
documented Cosmos values
!= durable Command Center runtime implemented
```

Generic 0.1.0 currently requires local Manager.

## NEXT audit target

Target operational topology agreed for the next stage:

```text
Web environment    local
Storage            final/durable
Cosmos             local
Master Projection  available to both products
```

The audit must decide which variables are:

```text
manual
derived
secret
defaulted
DEV/UAT/PRD specific
```

Do not invent a new key simply to make documentation symmetrical.

## ADA authoring proposal

ADA already supports `ContentStatePresentationMode.AUTHORING`.

Exposing a local-only environment selector for that mode is **PROPOSED / NOT FROZEN**.

The next `.env.detail` audit may decide whether such a variable belongs in the distributed contract.

## Production

Secrets remain external to Git and `.env.detail`.

Key Vault/App Settings mapping is PLANNED and should be derived only after variable ownership is frozen.
