# ADA Command Center — Current Implementation

Estado: **CURRENT — PRODUCT HOST + DURABLE COMPOSITION + GENERIC STORAGE NAMESPACE + MASTER PROJECTION**

## Generic Application

```text
ada-command-center-generic-application==0.1.2
```

Role:

```text
real Command Center Web product composition root
```

Composition includes:

```text
Home
Identity
Projected Navigation
Navigation authorization
Manager surface/modules
Master Projection independent surface
Command Center pages
Manager pages
```

## Configuration Manager

Current package:

```text
ada-command-center-configuration-manager==0.1.3
```

Local Web host accepts:

```text
ADA_MANAGER_PERSISTENCE_PROVIDER=local
ADA_MANAGER_PERSISTENCE_PROVIDER=durable
```

Identity remains local in this host.

## Durable stores CURRENT

```text
Blob Source
Cosmos Alarm Configuration Projection
Cosmos Profiles Projection
Cosmos Navigation Projection
Blob Users Registry
Cosmos Users Runtime
Tool Catalog on Storage
```

## Storage namespace CURRENT

Command Center now depends on:

```text
atlanticus-web-storage-namespace==0.1.0
atlanticus.web.storage.namespace.StorageNamespace
```

Current product namespace:

```text
StorageNamespace(
    "conciencia_situacional",
    "command-center",
)
```

Preserved scope prefix:

```text
conciencia_situacional/command-center
```

The previous dependency on:

```text
ada.web.storage.namespace.AdaStorageNamespace
```

is superseded and removed.

## Master Projection CURRENT

Shared engine:

```text
atlanticus-web-master-projection==0.1.0
```

Product composition domains:

```text
Profiles
Navigation
Alarm Configuration
```

Product provisioning command:

```text
uv run ada-command-center-master-projection generate --user <service-user>
```

Derived material identity remains:

```text
conciencia_situacional/command-center/master-projection/material.zip
```

## Tool discovery CURRENT

```text
ada-command-center-web-tool-discovery-cosmos==0.1.1
```

Discovery uses the generic `StorageNamespace` when parsing persisted:

```text
<application>/<tool>
```

Tool Projection physical namespace identity is preserved.

## Catalog Manager CURRENT

```text
ada-command-center-web-tool-catalog-manager==0.1.2
```

## Qualification of namespace convergence

Relevant packages:

```text
atlanticus-web-storage-namespace                  15 passed
ada-web-tools-projection-local                     3 passed
ada-web-tools-projection-cosmos                    5 passed
ada-web-tools-persistence                         10 passed
ada-generic-application                          249 passed
ada-command-center-web-tool-discovery-cosmos      38 passed
ada-command-center-web-tool-catalog-manager        9 passed
ada-command-center-configuration-manager          31 passed
ada-command-center-generic-application            10 passed
```

Total:

```text
370 passed
```

Also verified:

```text
Ruff PASS
format PASS
commented mirrors equivalent
git diff --check PASS
legacy AdaStorageNamespace references = 0
legacy ada.web.storage.namespace imports = 0
9 package uv lock --check PASS
```

## NEXT

Actual dual-app durable runtime smoke remains:

```text
PLANNED / UNVERIFIED
```

It must validate the application-level composition with selected durable Storage/Cosmos targets, not only isolated package tests.
