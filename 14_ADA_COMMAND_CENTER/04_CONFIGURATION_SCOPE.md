# ADA Command Center — Configuration Scope

Estado: **CURRENT — LOCAL/DURABLE PERSISTENCE CONTRACT IMPLEMENTED**

## Existing configuration reader

`ManagerConfigurationReader` resolves:

```text
ATLANTICUS_ENVIRONMENT
ADA_MANAGER_PERSISTENCE_PROVIDER
ADA_COMMAND_CENTER_STORAGE_CONNECTION_STRING
ADA_COMMAND_CENTER_STORAGE_CONTAINER_NAME
ADA_COMMAND_CENTER_COSMOS_ENDPOINT
ADA_COMMAND_CENTER_COSMOS_DATABASE_NAME
ADA_COMMAND_CENTER_COSMOS_KEY
dynamic named Tool Cosmos connections
```

## Selector semantics

```text
ATLANTICUS_ENVIRONMENT
→ host/runtime behavior

ADA_MANAGER_PERSISTENCE_PROVIDER
→ local | durable persistence
```

Do not introduce Azure/emulator provider modes.

## Durable CURRENT

One Command Center Storage connection/container supports:

```text
Tool Catalog
Source
Users Registry
Master material
```

One own Command Center Cosmos connection/database supports:

```text
Alarm Configuration
Profiles
Navigation
Users Runtime
```

External Tool Cosmos connections remain independently named.

## Master Projection

No manual location variable.

Derived identity:

```text
conciencia_situacional/command-center/master-projection/material.zip
```

Command:

```text
uv run ada-command-center-master-projection generate --user <service-user>
```

## Production

Production identity remains separate and UNVERIFIED.

## NEXT ownership cleanup

Current reader defines:

```text
COMMAND_CENTER_NAMESPACE = AdaStorageNamespace('conciencia_situacional', 'command-center')
```

using a class owned by the ADA scope.

The next Source/namespace convergence must remove this product-to-product dependency without
changing connection semantics.
