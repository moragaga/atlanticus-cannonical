# Distribution and Tooling — Canonical Index

Estado: **CURRENT — ADA DISTRIBUTED LINUX RUNTIME CLOSED / PRODUCT CUTOVER NEXT**

## CURRENT layout

```text
/tooling/distribution/web
    reusable Web distribution mechanisms

/scopes/ada/tooling
    ADA-specific distribution composition/tooling

/scopes/ada-command-center/tooling
    Command Center-specific distribution composition/tooling
```

Web Distribution and Process Distribution remain separate boundaries.

## Closed prerequisites

```text
CURRENT-HEAD-DISTRIBUTION-REGENERATION
ADA-LOCAL-COSMOS-DATA-EXPLORER
ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE
ADA-CONSUMER-REPOSITORY-RUNTIME
```

## ADA current distribution

```text
ada-generic-application    0.2.26
ada-project-tooling        0.1.1
internal wheels            73
delivery strategy          internal-wheels-external-image-build
Docker/Linux runtime       VERIFIED
```

## Local operational tooling

Cosmos built-in Data Explorer is enabled on:

```text
127.0.0.1:${ADA_COSMOS_EXPLORER_PORT:-1234}
```

## Host sync limitation

macOS host `sync` is BLOCKED with current Python 3.14.2 binary-only requirements because `rcssmin==1.2.2` has no usable macOS CPython 3.14 wheel in the tested contract.

This does not invalidate Docker/Linux runtime qualification.

## NEXT

Distribution itself is not the next architecture focus.

Next product focus:

```text
ADA-TOOL-SCOPED-CONFIGURATION-AND-USER-RUNTIME
```

After implementation, regenerate the ADA distribution and rerun the consumer smoke.

## Deferred

```text
Command Center distributed runtime
Process Distribution
Python 3.14.7 / Trixie
production Azure/Entra
```
