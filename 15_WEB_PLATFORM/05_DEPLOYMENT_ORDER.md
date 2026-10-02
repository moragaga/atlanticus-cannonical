# Web Platform — Deployment Order

Estado: **CURRENT DIRECTION**

## General sequence

```text
0. external/base infrastructure
1. Web host
2. required resource availability/preparation
3. Source availability
4. Master Projection / product projection readiness
5. application operational readiness
6. backend producers/jobs
```

This is a lifecycle direction, not a global ordering between independent projection domains.

## Current local qualification target

After Source convergence:

```text
ADA Generic
ADA Command Center Generic
```

will be lifted with:

```text
ATLANTICUS_ENVIRONMENT=local
persistence=durable
```

Connection values determine whether Storage/Cosmos point to local emulators or Azure services.

## Runtime-smoke requirements

For each product:

```text
configuration parses
required resources are available
host starts
Master material can be provisioned/read
projection planner can inspect
selected apply flow can be exercised where meaningful
state survives the intended restart boundary
```

## Non-goals of next Source increment

Do not perform this smoke until the shared Source/namespace ownership gap is closed.

Do not mix backend KPI/Alarm jobs, production Entra or tooling topology normalization into that
Source change.
