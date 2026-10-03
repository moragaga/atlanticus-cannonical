# ADA Web — Current Baseline

Estado: **CURRENT — TOOL CONTRACT WEB CUTOVER CLOSED / DISTRIBUTION PREPARATION NEXT**

## Application

ADA Generic is the product composition root.

Version observed before the next distribution regeneration:

```text
0.2.26
```

No new distributed version was produced during the Tool Contract Web Cutover hito.

## Runtime qualification previously observed

From the isolated consumer repository:

```text
Web container              healthy
/health/live               HTTP 200
/health/ready              HTTP 200
environment                local
Cosmos database            visible
Cosmos containers          visible
Cosmos Data Explorer       HTTP 200
```

`/health/ready` still reports `checks: {}`.

These observations belong to the previous distributed runtime and were not requalified as a new distribution in this hito.

## Tool contract ownership CURRENT

Shared ADA Tool structural/source contracts now have one transversal owner:

```text
scopes/ada-contracts/tools
ada-contracts-tools==1.0.0
ada.contracts.tools
```

Web-specific Tool Configuration remains:

```text
scopes/ada/web/tools/configuration
```

Boundary:

```text
ada.contracts.tools
        |
        v
ada.web.tools.configuration
        |
        +--> Source / Projection
        +--> Branding
        +--> persistence
        +--> editor / callbacks / presentation
```

The ADA Generic release-chain no longer resolves `ada-web-tools`.

Runtime export gate:

```text
ada-contracts-tools  PRESENT
ada-web-tools        ABSENT
```

## Qualification of the cutover

Local qualification:

```text
ada-contracts/tools                 10 passed
ada-web-tools-configuration         75 passed
projection-local                     3 passed
projection-cosmos                    5 passed
ada-web-tools-persistence           10 passed
ada-configuration-manager           64 passed
ada-generic-application            205 passed
release-chain ownership scan       PASS
runtime dependency gate            PASS
```

Base remote commit used:

```text
moragaga/atlanticus@df2a125cf428085419595d8ad164fce0f8d86115
```

The qualified result is still a local working tree until integrated. Do not present it as the current remote `main` commit.

## Commented mirrors

Commented source remains valid as pedagogical material.

Tests whose only purpose was to assert structural/AST/token equivalence between productive and `commented` were retired.

Final local audit:

```text
No commented-mirror tests remain: PASS
```

This does not mean the full monorepo test suite was executed; only the suites explicitly reported in the cutover qualification are verified.

## Existing operational gaps not changed by this hito

### Header

Tool display name belongs to Tool Configuration and is wired to operational branding.

Dynamic refresh after Tool reprojection was not changed in this hito.

### Time Status

PI and Dispatch remain modeled and labeled in UI contracts.

The runtime timestamp/source feed was not addressed in this hito.

### Navigation

Navigation behavior was not modified or requalified in this hito.

Do not infer new navigation behavior from the Tool contract cutover.

## Retirement still BLOCKED

Physical removal of:

```text
scopes/ada/web/tools/core
```

is blocked by references outside this chat scope, principally Command Center locks/configuration.

The directory is not the accepted owner for new Web consumers.

Inspection locks under:

```text
scopes/ada/web/inspection/portability
scopes/ada/web/inspection/providers/kpi-definition
```

remain outside this hito because regeneration is independently blocked by an invalid KPI Definition project path.

## Current priority

The boundary that blocked a new Generic release is closed.

Single next focus:

```text
ADA Generic artifact generation qualification
```

That focus should:

```text
regenerate the expected artifacts
validate that all artifact generation paths succeed
audit .env.detail as part of the generated artifact contract
identify which environment values are user-supplied vs system-derived
```

Do not mix that increment with Command Center cleanup, physical retirement of `tools/core`, alarms, timeseries or unrelated UI work.

Only after artifact generation and `.env.detail` qualification should the final distribution regeneration/isolated-consumer exercise become the next increment.
