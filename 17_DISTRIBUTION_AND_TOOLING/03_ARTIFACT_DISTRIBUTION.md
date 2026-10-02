# Artifact and Distribution Boundary

Estado: **CURRENT CONTRACT / WEB ARTIFACT REGENERATION CLOSED / ADA LINUX RUNTIME UNVERIFIED**

## Boundary

Web Distribution and Process Distribution are separate boundaries.

### Web Distribution

```text
SOURCE PACKAGES
    ↓
PRODUCT COMPOSITION
    ↓
SCOPE TOOLING
    ↓
REUSABLE ROOT WEB DISTRIBUTION MECHANISMS
    ↓
WEB ARTIFACT
    ↓
HOST / DEVOPS / RUNTIME
```

Primary implementation surface:

```text
tooling/distribution/web
scopes/ada/tooling/distribution/web
scopes/ada-command-center/tooling/distribution/web
```

### Process artifacts/distribution

```text
deployment/processes/bundle.py
tooling/local/processes/process.py
tooling/distribution/processes
```

Process artifacts were not regenerated or requalified in this hito and must not be inferred from Web Distribution evidence.

## Current Web artifact evidence

### Generic

```text
packages          36
status            PASS
qualification     PORTABLE
readiness         ready
```

Qualification exercised:

```text
health.live
health.ready.diagnostic
home.http
dash.layout
```

This is real portable-runtime qualification for the generated Generic artifact on the generation platform.

### ADA

```text
delivery strategy    internal-wheels-external-image-build
internal wheels      73
status               PRECHECK_PASS
image_build          UNVERIFIED
runtime              UNVERIFIED
```

Current root packages include:

```text
ada-generic-application==0.2.26
ada-project-tooling==0.1.1
atlanticus-web-storage-preparation==0.1.0
```

The artifact was regenerated after the current ADA runtime/distribution contract corrections.

### Command Center

```text
packages             92
status               PRECHECK_PASS
qualification        PORTABLE precheck
dependency_check     PASS
runtime              UNVERIFIED
```

The Command Center artifact includes the shared durable preparation capability transitively through its current dependency graph.

## Current portability contract

Generic and Command Center wheelhouse artifacts reject editable internal package dependencies during portable qualification.

ADA intentionally uses a different delivery strategy:

```text
internal wheels
+
hash-pinned external runtime requirements
+
image build outside the monorepo
```

Therefore ADA `PRECHECK_PASS` proves artifact integrity/preflight only. It does not prove Linux image build or runtime.

## Current ADA distribution invariants

```text
ADA Starter pins ada-generic-application==0.2.26
ADA project tooling version is 0.1.1
ADA internal wheel count is 73
ADA physical durable contract uses ADA_STORAGE_* / ADA_COSMOS_*
ADA local Compose default Storage container is dataproduct
legacy ADA physical/provider env aliases are not supported
```

## Qualification semantics

```text
PASS
    only where the declared runtime qualification actually ran

PRECHECK_PASS
    artifact structure / dependency / integrity contract passed
    runtime remains unverified unless separately exercised

UNVERIFIED
    no evidence produced in this hito
```

Do not promote `PRECHECK_PASS` to runtime verification.

## Repository-global state

Artifact manifests retain source Git HEAD as provenance.

A global repository hash, clean working tree or unrelated-scope diff is not a qualification gate. Independent scopes can advance concurrently; gates must target the files/contracts owned by the increment.

## OPEN

```text
ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE
```

Required evidence:

```text
artifact copied outside monorepo
Linux image builds from artifact
Azurite + Cosmos Emulator start
durable resources prepare successfully
ADA Web starts
/health/live responds correctly
/health/ready is inspected
runtime has no monorepo dependency
restart/readback is validated where required by the smoke contract
```

Command Center distributed runtime remains separately UNVERIFIED.
