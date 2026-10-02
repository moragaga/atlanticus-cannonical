# Distribution and Tooling — Source Ledger

Estado: **AUDIT LEDGER — DISTRIBUTION CONTRACT CONVERGENCE CLOSED 2026-10-02**

## Authorities inspected for this closure

```text
Implementation
moragaga/atlanticus:main
c5565e36409a50e2d193e15820c886b75aa25539

Decisions
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical before these replacements
moragaga/atlanticus-cannonical:main
25b4ce885e195bd9c8d54f4077615ca936e9ecbf
```

Git remained read-only from the assistant side during this closure.

Repository-global HEAD is provenance only and is not a future qualification gate.

## Implemented delta closed by this hito

### Shared/current distribution graph

```text
generic / ada / command-center product catalog
shared Web distribute/generate/build/qualify/probe engine
ADA distribution under scopes/ada
Command Center distribution under scopes/ada-command-center
shared durable resource preparation capability
current StorageNamespace ownership
```

### ADA physical configuration convergence

SUPERSEDED:

```text
ADA_MANAGER_PERSISTENCE_PROVIDER
ADA_TOOL_SOURCE_PROVIDER
ADA_TOOL_PROJECTION_PROVIDER
ADA_TOOL_SOURCE_BLOB_*
ADA_TOOL_PROJECTION_COSMOS_*
```

CURRENT:

```text
ADA_PERSISTENCE_MODE
ADA_APPLICATION_NAMESPACE
ADA_TOOL_NAMESPACE

ADA_STORAGE_CONTAINER_NAME
ADA_STORAGE_CONNECTION_STRING
ADA_STORAGE_ACCOUNT_URL
ADA_STORAGE_SAS_TOKEN

ADA_COSMOS_ENDPOINT
ADA_COSMOS_KEY
ADA_COSMOS_DATABASE_NAME
```

No compatibility aliases were retained.

Default ADA durable Storage container:

```text
dataproduct
```

### ADA distributed Compose/tooling convergence

Current distributed Compose uses the application-level durable contract.

Current ADA project tooling validates:

```text
ADA_PERSISTENCE_MODE=durable
ADA_TOOL_NAMESPACE=<resolved logical namespace>
```

instead of the removed provider selectors.

Package versions after the fixes:

```text
ada-generic-application==0.2.26
ada-project-tooling==0.1.1
```

The ADA Starter pins:

```text
ada-generic-application==0.2.26
```

### ADA local resource preparation correction

`local_resources.py` and its pedagogical mirror consume the current `AdaGenericSettings` API:

```text
cosmos_endpoint
storage_connection_string
storage_container_name
```

The superseded internal attribute names are no longer the expected runtime contract.

### Distribution boundary test refinement

The old test that required ADA-owned physical implementation files for shared Master Projection runtime was SUPERSEDED.

The current boundary test validates the dependency/composition contract instead of freezing internal file placement.

This aligns with the project testing rule: test behavior/contracts/invariants, not implementation structure.

## Qualification evidence

### ADA Generic

```text
full pytest suite     PASS
Ruff check            PASS
Ruff format           PASS
uv lock               resolved / current
```

### Web Distribution tests

Final evidence after convergence:

```text
Compose integration focused    13 passed
tooling/distribution/web        130 passed
Ruff                            PASS
format                          PASS
contract validation             PASS
```

### Current Web artifacts

```text
generic
  packages            36
  status              PASS
  qualification       PORTABLE
  readiness           ready

ada
  internal wheels     73
  status              PRECHECK_PASS
  image_build         UNVERIFIED
  runtime             UNVERIFIED
  ada-generic         0.2.26
  project-tooling     0.1.1

command-center
  packages            92
  dependency_check    PASS
  status              PRECHECK_PASS
  runtime             UNVERIFIED
```

The final regenerated ADA artifact was explicitly scanned for:

```text
ADA_MANAGER_PERSISTENCE_PROVIDER
ADA_TOOL_SOURCE_PROVIDER
ADA_TOOL_PROJECTION_PROVIDER
ADA_TOOL_SOURCE_BLOB_*
ADA_TOOL_PROJECTION_COSMOS_*
```

and returned zero matches.

## Configuration templates

CURRENT:

```text
ADA              DEV/UAT/PRD mappings + secret references
Command Center   DEV/UAT/PRD mappings + secret references
Generic          no configuration templates
```

`dataproduct` is the current Storage container convention/default for ADA Generic and Command Center deployment contracts.

## Superseded findings/paths

```text
historical current-head artifacts marked UNVERIFIED
SUPERSEDED by current regeneration evidence

historical ADA 0.2.22 / 0.2.23 / 0.2.24 / 0.2.25 canonical references
SUPERSEDED by ada-generic-application==0.2.26

historical ada-project-tooling==0.1.0
SUPERSEDED by 0.1.1

Tool-specific physical Storage/Cosmos ADA variable naming
SUPERSEDED by application-level ADA_STORAGE_* / ADA_COSMOS_*

provider-selector-based ADA distributed Web Compose validation
SUPERSEDED by ADA_PERSISTENCE_MODE + ADA_TOOL_NAMESPACE

implementation-file-existence Master Projection boundary test
SUPERSEDED by dependency/composition contract test
```

## Decisions alignment

No textual decision artifact indexed in `atlanticus-decisions:main` was found that defines the new ADA distribution variable names or the `ada-project-tooling` version.

Therefore:

```text
explicit decisions-repository conflict    UNVERIFIED / none identified
implementation vs old canonical           CONFLICT / canonical stale
```

Binary historical decision documents were not reinterpreted as authority for undocumented current distribution names.

## Next audit boundary

```text
ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE
```

No KPI, Process Distribution, Command Center feature work or Python migration belongs to that next increment.
