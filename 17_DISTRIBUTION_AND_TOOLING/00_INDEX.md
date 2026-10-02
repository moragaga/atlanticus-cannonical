# Distribution and Tooling — Canonical Index

Estado: **CURRENT — WEB DISTRIBUTION REGENERATION CLOSED / ADA DISTRIBUTED LINUX RUNTIME SMOKE NEXT**

## CURRENT layout

```text
/tooling/distribution/web
    reusable Web distribution mechanisms
    product catalog
    base starter

/scopes/ada/tooling
    ADA-specific distribution composition
    ADA distributed project tooling

/scopes/ada-command-center/tooling
    Command Center-specific distribution composition
```

Operational Data owns its processes under `scopes/operational-data`. Process artifact/distribution tooling is a separate boundary from Web Distribution and was not part of this hito.

## Closed prerequisites

The following prerequisites are CLOSED / VERIFIED:

```text
SOURCE-NAMESPACE-AND-COMPOSITION-CONVERGENCE
SHARED-DURABLE-RESOURCE-PREPARATION
DUAL-APP-DURABLE-RUNTIME-SMOKE
DISTRIBUTION-CONTRACT-CONVERGENCE
ADA-COMPOSE-CONTRACT-CONVERGENCE
ADA-PROJECT-TOOLING-CONTRACT-CONVERGENCE
ADA-LOCAL-RESOURCES-CONTRACT-CONVERGENCE
CURRENT-HEAD-DISTRIBUTION-REGENERATION
```

Generic capabilities relevant to the current distribution include:

```text
atlanticus-web-storage-namespace==0.1.0
atlanticus-web-storage-preparation==0.1.0
atlanticus-web-master-projection==0.1.0
```

## Current Web product state

### Generic

```text
packages          36
status            PASS
qualification     PORTABLE
readiness         ready
runtime checks    health.live
                  health.ready.diagnostic
                  home.http
                  dash.layout
```

### ADA

```text
ada-generic-application    0.2.26
ada-project-tooling        0.1.1
internal wheels            73
status                     PRECHECK_PASS
image_build                UNVERIFIED
runtime                    UNVERIFIED
```

The ADA artifact contains no active references to the superseded physical/provider variables:

```text
ADA_MANAGER_PERSISTENCE_PROVIDER
ADA_TOOL_SOURCE_PROVIDER
ADA_TOOL_PROJECTION_PROVIDER
ADA_TOOL_SOURCE_BLOB_*
ADA_TOOL_PROJECTION_COSMOS_*
```

### Command Center

```text
ada-command-center-configuration-manager    0.1.4
ada-command-center-generic-application      0.1.3
packages                                    92
dependency_check                            PASS
status                                      PRECHECK_PASS
runtime                                     UNVERIFIED
```

## Current configuration-template state

```text
generic          configuration templates: false
ADA              configuration templates: true
Command Center   configuration templates: true
```

ADA and Command Center generate deployment mappings for DEV/UAT/PRD from their current `.env.detail` contracts.

## Qualification rule

`PRECHECK_PASS` is not runtime verification.

Artifact manifests may record a repository HEAD for provenance, but repository-global HEAD or clean-tree state is not a qualification gate because independent scopes can advance concurrently. Qualification must be tied to the owned files, contracts and artifact inputs of the increment.

## NEXT

The unique next project focus is:

```text
ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE
```

It must prove ADA from a copy of the generated distribution outside the monorepo, including Linux image build, isolated local durable infrastructure, resource preparation, Web startup, health checks and absence of monorepo dependency.

## Deferred / separate

```text
Command Center distributed runtime qualification
Process artifact/distribution regeneration
KPI Runtime
KPI Historian
KPI Delivery / Timeseries
Collector
browser stores
UI
production Azure / Entra qualification
Python 3.14.7 / Trixie migration
tooling topology normalization
```
