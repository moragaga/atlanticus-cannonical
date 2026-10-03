# Alarm Engine — Command Center / Modeler / Delivery / Web / Analytics Boundary

Estado: **REFINED / TARGET DESIGN FROZEN; physical implementation still under Command Center backend**.

## 1. Principle

Command Center owns configuration authoring, semantic validation and publication.

Alarm Engine owns backend execution and state:

```text
core
materialization
persistence
runtime
modeler
delivery
```

Web consumes modeled projections/results and must not become an Engine dependency.

Analytics builds read models from durable facts and does not mutate Engine operational state.

## 2. Current physical state

CURRENT:

```text
scopes/ada-command-center/backend
```

contains:

```text
alarms/core
alarms/materialization
alarms/persistence
processes/alarms-materialization
processes/alarms-runtime
processes/alarms-delivery
```

No `alarms-modeler` process exists yet.

## 3. Target direction

```text
ADA Command Center
    authoring
    business validation
    Tool/reference validation/resolution
    routing/visual validation
    publication
        ↓
shared published Alarm contract
        ↓
ADA Alarm Engine
    materialization
    persistence
    runtime
    modeler
    delivery
        ↓
projection store / durable modeled heads
        ↓
Command Center Web

Engine durable facts
        ↓
History / Analytics
```

Dependency direction is one-way across publication/consumption boundaries.

## 4. Materialization target

```text
published Alarm configuration
        ↓
deterministic split
        ├── RuntimeConfiguration
        ├── ModelerConfiguration
        └── DeliveryConfiguration
```

It must not:

```text
discover Tools
query Command Center Tool Catalog
import Configuration Manager
import Command Center Web projection packages
repeat semantic resolution closed before publication
```

## 5. Runtime

Runtime owns:

```text
evaluation
occurrence / episode
priority semantics
management/deactivation runtime effects
assignments
adoption/effective configuration
durable execution facts
```

Runtime does not know carousel/queue-in-queue slots or physical publication destinations.

## 6. Modeler

Modeler is Engine backend logic, not Web.

It owns logical presentation state that requires memory/time:

```text
slot membership
ordering
visible/hidden queues
carousel rotation
queue-in-queue rotation
timer anchors
reconciliation
checkpoint/recovery
modeled heads
```

It may know logical `component_key`, `subcomponent_key`, logical slot number and projection mode because these are part of the projection contract.

It must not know:

```text
CSS
Dash callback graph
pixel dimensions
browser sessions
Web component instances
```

This refines older wording that the Engine does not know “layout”: Engine Core/Runtime do not know UI layout; Modeler may know **logical projection positions**, not Web geometry.

## 7. Delivery

CURRENT implementation consumes Runtime CURRENT/FACTS directly.

Target Delivery:

```text
consumes already modeled heads/documents
publishes them to resolved destinations
handles connection/retry/batching/transport concerns
```

Delivery does not decide ordering, dwell time, movement or queue semantics.

## 8. Web

Web:

```text
reads published modeled state
renders it
owns CSS/theme/pixel layout
```

Web does not:

```text
read WAL
run carousel timers
run queue-in-queue fairness
reconstruct operational state
```

## 9. Analytics

Analytics consumes durable Engine facts such as:

```text
Occurrence/Episode
Journey
Evidence
management/deactivation
routing/assignment
priority transitions
configuration revisions
```

Keep separate:

```text
Live modeled projection
Management modeled projection
History/Analytics
```

Modeled projection behavior must not become a prerequisite for preserving operational history.

## 10. Backpressure boundary

```text
Runtime never waits Modeler.
Modeler never waits Delivery for its state commit.
One Delivery destination never blocks other destinations.
```

Pending work lives in durable state, not unbounded process memory.

## 11. Known CURRENT inverted dependencies

Materialization acquisition still depends on Web/projection packages.

These edges are **TO REMOVE / INVERT**, not contracts to preserve.

## 12. Residual `domain/alarms`

Target direction:

```text
routing policy -> Command Center pre-publication validation
source key -> shared/primitive publication identity
```

Physical cleanup remains OPEN until import inventory.

## 13. History of the direct Delivery receiver

The current receiver is valid implementation evidence.

Its existence does not freeze the target architecture.

Target transition:

```text
Runtime -> Delivery     CURRENT implementation
Runtime -> Modeler -> Delivery   target
```

The first becomes SUPERSEDED only as a target contract until implementation cutover.
