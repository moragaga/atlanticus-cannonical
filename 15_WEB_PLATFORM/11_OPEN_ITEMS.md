# Web Platform — Open Items

Estado: **CURRENT — ADA DISTRIBUTED LINUX RUNTIME SMOKE NEXT**

## CLOSED / VERIFIED

```text
atlanticus-web-master-projection extraction
ADA adoption of shared Master engine
Command Center adoption of shared Master engine
Command Center durable Manager composition
SOURCE-NAMESPACE-AND-COMPOSITION-CONVERGENCE
SHARED-DURABLE-RESOURCE-PREPARATION
DUAL-APP-DURABLE-RUNTIME-SMOKE
DISTRIBUTION-CONTRACT-CONVERGENCE
ADA-COMPOSE-CONTRACT-CONVERGENCE
ADA-PROJECT-TOOLING-CONTRACT-CONVERGENCE
ADA-LOCAL-RESOURCES-CONTRACT-CONVERGENCE
CURRENT-HEAD-DISTRIBUTION-REGENERATION
```

## Current artifacts

```text
Generic
    PASS
    36 packages

ADA
    PRECHECK_PASS
    73 internal wheels
    ada-generic-application==0.2.26
    ada-project-tooling==0.1.1
    image_build=UNVERIFIED
    runtime=UNVERIFIED

Command Center
    PRECHECK_PASS
    92 packages
    dependency_check=PASS
    runtime=UNVERIFIED
```

## OPEN / NEXT

```text
ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE
```

Required:

```text
use the generated ADA artifact as the only application source
copy it outside the monorepo
build its Linux image
start isolated Cosmos Emulator + Azurite
configure ADA_TOOL_NAMESPACE
use ADA_PERSISTENCE_MODE=durable
use ADA_STORAGE_* / ADA_COSMOS_*
use dataproduct unless the smoke deliberately overrides the local container
prepare durable resources
start Web
validate /health/live
inspect /health/ready
prove no monorepo/editable/path dependency
validate restart/readback where the smoke contract requires it
```

Do not mix KPI, Process Distribution or Command Center work into this increment.

## OPEN / AFTER

After the ADA distribution is proven at runtime, resume one focused backend/process frontier at a time:

```text
Process artifact/distribution regeneration where needed
KPI Runtime
KPI Historian
KPI Delivery / Timeseries
Collector
browser stores
UI
```

The exact order after the ADA smoke must be chosen from current source authority at that time; this document does not promote all of them into one increment.

## OPEN / SEPARATE

```text
Command Center distributed runtime qualification
production Entra
Azure production qualification
production Key Vault/App Settings runtime evidence
Python 3.14.7 / Trixie
scope tooling topology normalization
Users destructive recovery
other historical Web gates not revalidated here
```

## Superseded NEXT markers

The following historical NEXT markers are SUPERSEDED:

```text
Source/namespace ownership as NEXT
dual-app durable runtime smoke as NEXT
current-head Web distribution regeneration as NEXT
```

The current unique NEXT is the ADA distributed Linux runtime smoke.
