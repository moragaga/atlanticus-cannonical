# ADA Command Center — Configuration Scope

Estado: **CURRENT IMPLEMENTATION + CONFIGURATION CONTRACT REVIEW NEXT**

## Existing configuration reader

`ManagerConfigurationReader` currently resolves:

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

## Current runtime reality

Generic 0.1.0 requires:

```text
environment != production
manager_provider == local
```

Storage is required for Tool Catalog.

Own Command Center Cosmos is reserved/configurable but not activated by the local product launcher as a durable Manager host.

## `.env.detail` status

The current file is **NOT YET FROZEN** for the next deployment target.

The next audit must decide:

```text
which persistence selector is canonical
which values are manual vs derived
which values are secrets
Storage final contract
Cosmos local contract
DEV/UAT/PRD mapping
Master Projection identity/configuration
```

Do not rename/remove `ADA_MANAGER_PERSISTENCE_PROVIDER` before the contract and implementation change are agreed; it is still CURRENT implementation.

## Master Projection

Requirement agreed for Command Center.

No current Master Projection implementation exists in the product runtime.

Target rule:

```text
Master Projection
→ product runtime capability
-X-> distribution tooling
```

If common implementation is extracted, it must be generic/reusable rather than importing ADA Generic runtime.

## Open production contracts

```text
production identity
durable Users/Profiles/Navigation topology
durable Manager host
resource preparation/startup gate
Azure/Entra
```

These remain separate from the immediate `.env.detail` audit unless required to define a configuration key correctly.
