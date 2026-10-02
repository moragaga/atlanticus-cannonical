# Source Storage — Implementation Order

Estado: **CURRENT PLAN — GENERIC CORE CLOSED / CROSS-PRODUCT CONSUMER CONVERGENCE NEXT**

## Closed foundation

```text
1. Source Core
2. Local provider
3. Blob provider
4. Projection exact-release Core
5. Manager generic Source/Projection handoff
```

Do not reopen these contracts without a concrete failing consumer.

## NEXT increment

```text
SOURCE-NAMESPACE-AND-COMPOSITION-CONVERGENCE
```

Order:

```text
1. inventory AdaStorageNamespace consumers
2. inventory Source composition paths in ADA and Command Center
3. separate generic namespace responsibility from product-specific naming
4. define the minimal reusable contract
5. replace cross-product imports cleanly
6. qualify ADA + Command Center affected suites
7. confirm Source Core behavior remains unchanged
```

## Explicit non-goals

```text
no SourceStore API change
no release/concurrency redesign
no retention/GC work
no tooling topology reorganization
no KPI/Collector/UI
no production identity
```

## After close

```text
DUAL-APP-DURABLE-RUNTIME-SMOKE
```
