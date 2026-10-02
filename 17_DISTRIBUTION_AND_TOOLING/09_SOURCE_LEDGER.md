# Distribution and Tooling — Source Ledger

Estado: **AUDIT LEDGER — WEB TOOLING CLEANUP CLOSED 2026-10-02**

## Authorities

```text
Implementation
moragaga/atlanticus@2dc5862f634eb0bf8fe72d771d56605d1c7f32cf

Decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical inspected before replacements
moragaga/atlanticus-cannonical@852e031d020edd4fdd5ab0187e95fbf6f443083b
```

Git remained read-only from the assistant side.

## Implemented delta

Closed changes include:

```text
central product catalog: generic / ada / command-center
shared distribute/generate/build/qualify/probe engine
ADA distribution support moved under scopes/ada
ADA starter moved under scopes/ada
Command Center starter moved under scopes/ada-command-center
Master Projection material/reader/provision moved into ada-generic-application
local_resources moved into ada-generic-application
ADA host/runtime centralized in ada-generic-application
base Starter example demo removed
cross-platform SHA256-locked sdist fallback added to shared wheelhouse builder
```

## Qualification evidence reported by user

```text
compose integration focused
10 passed, 3 skipped

tooling/distribution/web
125 passed, 3 skipped

Ruff check
PASS

Ruff format --check
PASS

git diff --check
PASS
```

## Distribution evidence

```text
generic
  packages            36
  status              PASS
  qualification       PORTABLE
  readiness           ready

ada
  internal wheels     71
  status              PRECHECK_PASS
  image_build         UNVERIFIED
  runtime             UNVERIFIED

command-center
  packages            85
  dependency_check    PASS
  status              PRECHECK_PASS
  runtime             UNVERIFIED
```

## macOS portability finding

Initial Generic/Command Center wheelhouse builds blocked because no compatible prebuilt `rcssmin==1.2.2` wheel was selected for macOS.

Root correction:

```text
locked wheel preferred
locked sdist fallback
hash-constrained build dependencies
wheel-only final artifact
source/output hashes recorded
```

After correction both distributions qualified to their declared contracts.

## Superseded paths/contracts

```text
tooling/distribution/web/ada
SUPERSEDED

tooling/distribution/web/starter/ada
SUPERSEDED

tooling/distribution/web/starter/command-center
SUPERSEDED

Starter-owned ADA Master Projection/runtime
SUPERSEDED

ADA_MASTER_PROJECTION_MATERIAL_PATH
SUPERSEDED
```

## Next audit boundary

```text
ADA + Command Center .env.detail
```

No new distribution refactor is planned unless the configuration/runtime audit reveals a concrete blocker.
