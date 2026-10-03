# env.detail Contract

Estado: **CURRENT PHYSICAL CONTRACT / COMPLETE CONTENT AUDIT PLANNED**

## Core ADA fields CURRENT

```text
ATLANTICUS_ENVIRONMENT
ADA_PERSISTENCE_MODE
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE
```

Durable:

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

## Namespace semantics

```text
ADA_APPLICATION_NAMESPACE
    global identity boundary

ADA_TOOL_NAMESPACE
    Tool Blob Source/membership/recovery boundary
```

## Cosmos CURRENT

```text
one Cosmos runtime/database per Tool
```

No Users routing env variables.

## Planned audit

After artifact generation, inspect every `.env.detail` entry for:

```text
meaning
comment
required/optional
safe example/default
system-derived/system-assigned possibility
consumer
scope
secret classification
```

Do not invent missing values.
