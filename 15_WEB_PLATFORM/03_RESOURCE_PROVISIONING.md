# Web Platform — Resource Provisioning

Estado: **CURRENT — ADA PREPARATION EXISTS / COMMAND CENTER DURABLE STORES EXIST / PARITY UNVERIFIED**

## Principle

Resource preparation belongs to deployment/application composition, not to every backend job.

A capability declares physical requirements; product composition decides which resources are
required for that application.

## ADA

ADA Generic already has explicit resource preparation commands for its current durable topology.

That work remains CURRENT and is not reopened by the Source convergence.

## Command Center

Durable runtime now resolves and uses:

```text
Storage container
Command Center Cosmos database
alarm-configuration
profiles-projection
navigation-projection
users-runtime
```

with the partition contracts owned by each capability.

However this hito did not implement/qualify an explicit Command Center resource-preparation CLI
equivalent to ADA.

Therefore:

```text
COMMAND-CENTER-RESOURCE-PREPARATION-PARITY
OPEN / UNVERIFIED
```

It may become a runtime-smoke blocker only if the selected test environment lacks the required
resources.

## Namespace interaction

Resource preparation must consume the same generic Source/namespace contract selected by the next
increment; do not encode another product-specific path scheme in provisioning.

## Separate

Azure permissions, production identity and destructive migration remain outside the next Source
increment.
