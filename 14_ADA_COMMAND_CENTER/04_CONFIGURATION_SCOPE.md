# ADA Command Center — Configuration Scope

Estado: **CURRENT — LOCAL/DURABLE PERSISTENCE + GENERIC USERS PARITY IMPLEMENTED**

## Existing configuration reader

`ManagerConfigurationReader` resuelve:

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

No introducir Azure/emulator como provider modes.

## Durable CURRENT

Un Storage connection/container de Command Center soporta:

```text
Tool Catalog
Source
Users Registry
Tool Membership
Users Recovery
Master material
```

El Cosmos propio de Command Center soporta:

```text
Alarm Configuration
Profiles
Navigation
Users Runtime
```

External Tool Cosmos connections siguen nombradas independientemente.

## Storage namespace CURRENT

Autoridad:

```text
atlanticus.web.storage.namespace.StorageNamespace
```

Namespace de producto:

```text
StorageNamespace(
    "conciencia_situacional",
    "command-center",
)
```

La dependencia anterior hacia `ada.web.storage.namespace.AdaStorageNamespace` está SUPERSEDED.

## Users durable CURRENT

```text
Registry
conciencia_situacional/users/users.json.gz

Membership
conciencia_situacional/command-center/users/memberships.json.gz

Recovery
conciencia_situacional/command-center/users/recovery/...

Runtime
Cosmos users-runtime
```

## Master Projection

No usar variable manual para material location.

Derived identity:

```text
conciencia_situacional/command-center/master-projection/material.zip
```

Users recovery/snapshot catalog se inyectan como operación especial cuando durable runtime está configurado.

## Production

Production Entra y autorización productiva permanecen UNVERIFIED.

## Separate

La incompatibilidad Tools observada por `catalog`/`discovery-cosmos` no cambia estos contratos de configuración y no se corrige en este hito.
