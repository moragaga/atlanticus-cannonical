# Alarm Engine — Runtime Adoption and Effective Configuration

Estado: **CURRENT Runtime/EFFECTIVE implementation; direct Delivery receiver CURRENT but target boundary SUPERSEDED; Modeler target PLANNED**.

## 1. Exact adoption invariants — FROZEN

```text
READY != EFFECTIVE

AlarmResolutionKey =
    (alarm_configuration_revision, confirmed_tool_catalog_revision)

Exact artifact ref =
    (source_key, result_id, manifest_sha256, resolution_key)

WAL -> Durable Head -> group snapshots -> Materialized Head -> EFFECTIVE projection
```

Materialization creates READY/BLOCKED artifacts; Runtime adoption gives operational authority to the exact artifact.

No fallback to latest READY.

## 2. Runtime configuration adoption — CURRENT

`RuntimeLocalConfigurationReader` validates candidate/exact materialization and the explicit evaluator registry participates in executability.

Planner/current implementation handles:

```text
UNCHANGED
COMPATIBLE
ADDED
ENABLED
DISABLED
REMOVED
STRUCTURAL_RESET
REJECTED
```

Do not broaden compatibility rules inside the Modeler increment.

## 3. WAL / EFFECTIVE — CURRENT

Engine persistence remains authority for durable adoption and runtime recovery.

`runtime/state/effective-head.json` is a recoverable projection of durable authority.

Runtime reopens exact artifact and builds its session from that exact pin.

## 4. Runtime executable/process — CURRENT

Current process composition includes real:

```text
application
bootstrap
configured iteration
operational runner
cycle
persistence/recovery
CURRENT publisher
FACTS exporter
```

No Modeler process has been implemented in this hito.

## 5. Runtime publication — CURRENT

After required durable commits, Runtime publishes:

```text
CURRENT v1
FACTS v2
```

CURRENT may change even without lifecycle commit because current evaluation/evidence may change.

FACTS exports only durable commit records.

## 6. Direct Delivery receiver — CURRENT implementation

`processes/alarms-delivery` currently acts as independent receiver with its own lease/recovery/cursors.

It:

```text
validates EFFECTIVE projection/source
reopens exact materialization
stages CURRENT
receives FACTS chain
owns its consumption cursor
```

This remains valid evidence of current implementation.

## 7. Target refinement — Runtime → Modeler → Delivery

The direct receiver boundary is SUPERSEDED as target.

New target:

```text
Runtime exact artifact X
    ↓
ModelerConfiguration X + Runtime handoff X
    ↓
Modeled heads X
    ↓
DeliveryConfiguration X
    ↓
publish
```

Invariant:

```text
Runtime, Modeler and Delivery operate on the same exact artifact ref.
```

Do not combine Runtime output from artifact A with Modeler/Delivery configuration from B.

## 8. Runtime → Modeler backpressure — FROZEN semantics

```text
Runtime producer does not wait for Modeler acknowledgement.
Modeler owns its checkpoint.
Changes required by Modeler are durable and ordered.
Modeler reads bounded batches.
```

The physical handoff schema remains OPEN.

## 9. Modeler recovery — FROZEN principle

On restart:

```text
load Modeler durable state
load input checkpoint
replay only after checkpoint
reconcile timers/state against current time
produce present valid modeled head
```

Do not replay every missed rotation as a visible frame.

## 10. Artifact change with live model state — OPEN

Runtime adoption already has exact semantics.

Modeler still needs a contract for transitions such as:

```text
artifact A scheduler state/backlog
→ artifact B
```

The next increments must define preservation/reconciliation/reset rules without weakening Runtime adoption.

## 11. Current vs target qualification

CURRENT historical Runtime + direct Delivery gates remain evidence of the existing pipeline.

UNVERIFIED:

```text
Modeler adoption
Modeler persistence/recovery
Runtime→Modeler physical handoff
Modeler→Delivery modeled heads
four-process Docker execution
production deployment
```

Do not claim these are implemented until qualified.
