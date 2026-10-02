# ADA Generic — Configuration to Runtime

Estado: **CURRENT — CONFIGURATION CONTRACT IMPLEMENTED; ENV.DETAIL AUDIT NEXT**

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

## Persistence selection

```text
local
→ Local Tool Source
→ Local Tool Projection
→ local Manager stores
→ local Master Projection path

durable
→ Blob Tool Source
→ Cosmos Tool Projection
→ durable Manager
→ Blob Master Projection
```

Durable settings can be used under local Web environment.

This is the intended basis for the next smoke:

```text
ATLANTICUS_ENVIRONMENT=local
ADA_PERSISTENCE_MODE=durable
Storage=final
Cosmos=local
```

## Storage credentials

Supported contract:

```text
connection string
OR
account URL + SAS token
```

Do not configure both simultaneously.

## Master Projection identity

No manual material path is required by the current runtime.

Logical relative identity:

```text
master-projection/material.zip
```

Local path and Blob name are derived from `AdaStorageNamespace` and application namespace.

Historical `ADA_MASTER_PROJECTION_MATERIAL_PATH` contract is SUPERSEDED.

## KPI Delivery

KPI Delivery Cosmos is optional and independent from Tool Projection Cosmos.

If any KPI Delivery Cosmos setting is supplied, endpoint/key/database must be complete.

Collector attachment occurs only when:

```text
Tool Projection resolution == READY
and
KPI Delivery Cosmos configured
```

## NEXT

Audit `.env.detail` to document:

```text
manual
derived
optional
secret
local
production
DEV/UAT/PRD mapping
```

Do not add new variables merely for documentation convenience.
