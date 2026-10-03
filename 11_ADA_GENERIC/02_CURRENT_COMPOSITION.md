# ADA Generic — Current Composition

Estado: **CURRENT — PRODUCT COMPOSITION ROOT / DISTRIBUTED RUNTIME VERIFIED**

## Version CURRENT

```text
ada-generic-application==0.2.26
ada-project-tooling==0.1.1
Python == 3.14.2
```

## Composition root CURRENT

ADA Generic posee:

```text
settings
local/durable Manager composition
identity binding
Tool Projection resolution
operational render binding
ADA Master Projection composition/provisioning
KPI Collector attachment
Web runtime lifecycle
```

Reusable engines remain owned by Atlanticus generic capabilities.

## Durable runtime qualification

Verified from an isolated consumer repository:

```text
Linux Docker image build        PASS
Azurite                         PASS
Cosmos Emulator                 PASS
resource preparation            PASS
Web container healthy           PASS
/health/live                    HTTP 200
/health/ready                   HTTP 200
Cosmos Data Explorer            HTTP 200
```

`/health/ready` currently reports `checks: {}`.

## Persistence modes

```text
ADA_PERSISTENCE_MODE=local
ADA_PERSISTENCE_MODE=durable
```

A local environment may use durable persistence with emulators.

## Storage namespace CURRENT API

```text
StorageNamespace(
    application_namespace,
    scope_namespace,
)
```

ADA maps:

```text
application_namespace = ADA_APPLICATION_NAMESPACE
scope_namespace       = ADA_TOOL_NAMESPACE
```

## Current composition gap

Implementation CURRENT still creates:

```text
application_source
    Navigation
    Profiles
    ADA Access
    Operational

tool_source
    Tools
    KPI Registry
    KPI Definitions
```

The accepted next cutover moves all Tool-varying configuration to `tool_source`; only global user identity remains application-global.

This is `DECIDED / PLANNED`, not yet CURRENT implementation.

## Host sync gap

`tooling/project.py sync` on macOS CPython 3.14.2 is BLOCKED because the current binary-only external dependency set includes `rcssmin==1.2.2` without a usable macOS CPython 3.14 wheel.

Docker/Linux distribution remains verified.
