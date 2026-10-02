# ADA Command Center — Current Implementation

Estado: **CURRENT — PRODUCT HOST + DURABLE LOCAL COMPOSITION + MASTER PROJECTION**

## Generic Application

```text
ada-command-center-generic-application==0.1.1
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

## Runtime CURRENT

Local Web host rejects production identity mode but accepts:

```text
ADA_MANAGER_PERSISTENCE_PROVIDER=local
ADA_MANAGER_PERSISTENCE_PROVIDER=durable
```

Identity remains local in this host.

Durable Manager composition is delegated to
`ada-command-center-configuration-manager==0.1.2`.

## Durable stores CURRENT

```text
Blob Source
Cosmos Alarm Configuration Projection
Cosmos Profiles Projection
Cosmos Navigation Projection
Blob Users Registry
Cosmos Users Runtime
```

Tool Catalog uses Storage.

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

Derived material identity:

```text
conciencia_situacional/command-center/master-projection/material.zip
```

## Current ownership gap

Command Center configuration currently imports:

```text
ada.web.storage.namespace.AdaStorageNamespace
```

That dependency is the next architecture cleanup.

## Qualification

```text
Generic pytest     10 passed
Ruff               PASS
format             PASS
```

Actual external durable runtime smoke remains UNVERIFIED.
