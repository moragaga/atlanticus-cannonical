# Web Platform — Current Gaps

Estado: **CURRENT — WEB DISTRIBUTION REGENERATION CLOSED / ADA DISTRIBUTED LINUX RUNTIME PRIMARY GAP**

## CLOSED / CURRENT

```text
generic Source Core/Local/Blob
generic Storage Namespace
shared durable resource preparation
generic Manager compositions used by current products
Command Center durable Manager composition
shared Master Projection engine
ADA Master Projection product composition
Command Center Master Projection product composition
cross-product namespace ownership convergence
dual-app durable runtime smoke
Web distribution contract convergence
current Web artifact regeneration
ADA physical Storage/Cosmos contract convergence
ADA distributed Compose/tooling convergence
ADA local resource preparation contract convergence
```

## Current Web distribution state

```text
Generic
    PASS / portable runtime qualification
    packages: 36

ADA
    PRECHECK_PASS
    internal wheels: 73
    ada-generic-application: 0.2.26
    ada-project-tooling: 0.1.1
    image build: UNVERIFIED
    runtime: UNVERIFIED

Command Center
    PRECHECK_PASS
    packages: 92
    dependency_check: PASS
    runtime: UNVERIFIED
```

## Primary gap

```text
ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE
```

Reason:

ADA artifact integrity and precheck are verified, but the current distribution has not yet been proven as an independent Linux runtime outside the Atlanticus monorepo.

The smoke must establish:

```text
copy generated ADA artifact outside monorepo
build Linux image from the copied artifact
start Cosmos Emulator and Azurite
use durable ADA contract with dataproduct
prepare durable resources
start ADA Web
validate /health/live
inspect /health/ready
prove there is no editable/path dependency on the monorepo
validate restart/readback where required by the agreed smoke contract
```

## Secondary gaps after the ADA distributed smoke

Still OPEN, but not part of the next increment:

```text
Command Center distributed runtime qualification
Process artifact/distribution regeneration
KPI Runtime
KPI Historian
KPI Delivery / Timeseries
Collector
browser stores
UI
```

## Separate production gaps

```text
production Entra integration
Azure production qualification
production Key Vault/App Settings runtime evidence
```

## Deferred architecture

```text
Python 3.14.7 / Trixie migration
scope tooling topology normalization
backend distribution tooling normalization
Operational Data tooling relocation/normalization
```

These are not blockers for the ADA distributed Linux runtime smoke.
