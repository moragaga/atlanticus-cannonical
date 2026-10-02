# Distribution and Tooling — Canonical Index

Estado: **CURRENT — ROOT ORCHESTRATION + SCOPE OWNERSHIP; NORMALIZATION DEFERRED**

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

Operational Data already owns its processes under `scopes/operational-data`, while some
Operational Data gate/orchestration logic remains under root tooling.

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

No implementation in this hito.

## Artifact state

Historical:

```text
generic         PASS
ADA             PRECHECK_PASS
Command Center  PRECHECK_PASS
```

Current package versions changed after those artifacts.

Therefore current-head artifact qualification is:

```text
UNVERIFIED
```

## Priority

Tooling normalization is `PLANNED / DEFERRED`.

NEXT project focus is Source namespace/composition convergence, then lifting both apps.
