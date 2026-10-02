# Frontend Generation

Estado: **CURRENT — SHARED MECHANISMS / PRODUCT-SCOPE COMPOSITION**

## Shared engine

```text
tooling/distribution/web
```

owns reusable mechanisms:

```text
products catalog
starter generation
wheelhouse build
qualification/probe primitives
distribution orchestration
base starter
```

## ADA

```text
scopes/ada/tooling/distribution/web
```

owns ADA-specific distribution behavior.

Runtime authority remains `ada-generic-application`.

## Command Center

```text
scopes/ada-command-center/tooling/distribution/web
```

owns Command Center-specific starter behavior.

Runtime authority remains `ada-command-center-generic-application`.

## Master Projection

Master Projection engine is not tooling:

```text
web/capabilities/master-projection
```

Product tooling may package/invoke a product command, but must not reimplement reader/planner/
executor/runtime behavior.

## Planned topology refinement

Same scope ownership model should later extend to backend/process distribution.

No relocation is required before the next application smoke.
