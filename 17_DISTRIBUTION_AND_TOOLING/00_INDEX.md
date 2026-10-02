# Distribution and Tooling — Canonical Index

Estado: **CURRENT — ROOT ORCHESTRATION + SCOPE OWNERSHIP; CURRENT-HEAD ARTIFACT REGENERATION AFTER DURABLE SMOKE**

## CURRENT layout

```text
/tooling/distribution/web
    reusable Web distribution mechanisms
    product catalog
    base starter

/scopes/ada/tooling
    ADA-specific distribution composition

/scopes/ada-command-center/tooling
    Command Center-specific distribution composition
```

Operational Data owns its processes under `scopes/operational-data`, while some Operational Data gate/orchestration logic remains under root tooling.

## Architecture direction

```text
/scopes/<owner>/tooling
    owner-specific build/distribution/qualification composition

/tooling
    reusable mechanisms + cross-scope orchestration
```

Planned future normalization:

```text
scopes/operational-data/tooling
scopes/ada/tooling/distribution/backend
scopes/ada-command-center/tooling/distribution/backend
```

No normalization implementation is part of the namespace hito.

## Source namespace prerequisite — CLOSED

The previous distribution prerequisite:

```text
SOURCE-NAMESPACE-AND-COMPOSITION-CONVERGENCE
```

is now CLOSED / VERIFIED.

Current generic package added to the Web platform:

```text
atlanticus-web-storage-namespace==0.1.0
```

Current consumers no longer depend on the removed ADA-owned `ada-web-storage-namespace`.

## Artifact state

Historical artifacts:

```text
generic         PASS
ADA             PRECHECK_PASS
Command Center  PRECHECK_PASS
```

Current package versions and dependencies have changed since those artifacts.

Therefore current-head artifact qualification remains:

```text
UNVERIFIED
```

## Ordering

NEXT project focus:

```text
DUAL-APP-DURABLE-RUNTIME-SMOKE
```

After that:

```text
CURRENT-HEAD-DISTRIBUTION-REGENERATION
```

Distribution regeneration must use the current package graph, including:

```text
atlanticus-web-storage-namespace==0.1.0
ada-web-tools-projection-local==0.1.1
ada-web-tools-projection-cosmos==0.1.1
ada-web-tools-persistence==0.1.1
ada-generic-application==0.2.23
ada-command-center-web-tool-discovery-cosmos==0.1.1
ada-command-center-web-tool-catalog-manager==0.1.2
ada-command-center-configuration-manager==0.1.3
ada-command-center-generic-application==0.1.2
```

After distribution qualification:

```text
ADA-GENERIC-OVER-ATLANTICUS-DISTRIBUTION
```

must prove that ADA consumes Atlanticus artifacts without relying on editable monorepo paths.

## Deferred

```text
tooling topology normalization
Python 3.14.7 / Trixie migration
production Azure/Entra qualification
```
