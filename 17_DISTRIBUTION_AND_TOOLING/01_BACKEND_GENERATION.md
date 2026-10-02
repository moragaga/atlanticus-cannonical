# Backend Generation

Estado: **CURRENT DIRECTION / SCOPE TOOLING MODEL PLANNED**

## Boundary

Separate:

```text
backend/process implementation
scope-specific distribution composition
generic distribution mechanisms
external DevOps pipeline
```

## Ownership direction

For a distributable scope:

```text
scopes/<scope>/backend
    backend packages/processes

scopes/<scope>/tooling/distribution/backend
    scope-specific artifact composition/qualification
```

Root:

```text
/tooling
    reusable distribution mechanics
    cross-scope orchestration
```

Operational Data follows the same ownership principle even though its primary distributables are
processes rather than Web applications.

## CURRENT

ADA and Command Center already have backend trees.

Backend distribution tooling normalization is not implemented as part of the current Web/runtime
hito.

## PLANNED / DEFERRED

When backend artifacts become the active focus:

```text
ADA backend tooling
Command Center backend tooling
Operational Data scope tooling normalization
root orchestration contract
```

Define contracts before moving paths.

## DevOps boundary

Atlanticus owns:

```text
artifact + distribution contract
```

External DevOps owns:

```text
pipeline implementation/execution
```
