# Alarm Engine — Command Center / Web / Analytics Boundary

Estado: **REFINED / PLANNED — Engine extraction design NEXT; physical implementation not moved yet**.

## 1. Principle

Command Center owns configuration authoring and publication.

Alarm Engine owns backend execution, state, materialization, persistence, runtime and delivery.

Web consumes projections/results and must not become an Engine dependency.

Analytics builds read models from durable facts and does not mutate Engine state.

## 2. Current physical state

Today the Engine implementation is physically located under:

```text
scopes/ada-command-center/backend
```

Candidate Engine packages:

```text
alarms/core
alarms/materialization
alarms/persistence
processes/alarms-materialization
processes/alarms-runtime
processes/alarms-delivery
```

This physical location is CURRENT.

## 3. Working ownership hypothesis

PROPOSED for next design:

```text
all current Alarm backend packages belong to ada-alarm-engine
```

The extraction should not cherry-pick only pure core.

Instead, remove/invert the layers that point from backend into Command Center Web.

## 4. Target direction

```text
ADA Command Center
    authoring
    business validation
    Tool/reference validation
    routing/visual validation
    publication
        ↓
shared published Alarm contract
        ↓
ADA Alarm Engine
    materialization
    persistence
    runtime
    delivery
        ↓
durable operational facts / projections
        ↓
Command Center Web / Analytics
```

Dependency direction is one-way across the publication boundary.

## 5. Materialization

Target responsibility:

```text
published Alarm configuration
        ↓
contract validation
        ↓
deterministic split/materialization
        ├── runtime artifact
        └── delivery artifact
```

It must not:

```text
discover Tools
query Command Center Tool Catalog
import Configuration Manager
import Command Center Web Alarm projection
depend on atlanticus.web.source/projection only to understand Engine configuration
repeat semantic resolution already closed before publication
```

## 6. Runtime

Runtime owns:

```text
evaluation
lifecycle
priority
management/deactivation runtime effects
adoption/effective configuration
durable execution facts
```

Runtime does not know layout or Web consumers.

## 7. Persistence

Engine persistence owns its durable operational state/WAL and recovery semantics.

Its physical implementation may use generic Atlanticus backend/connectivity capabilities, but it must not depend on Command Center Web.

## 8. Delivery

Delivery consumes Engine state/facts and the exact delivery configuration matching the same effective artifact.

It may carry stable semantic routing keys, but not layout geometry or CSS.

## 9. Web

Web decides:

```text
layout
visual composition
CSS/theme
presentation
```

Engine decides:

```text
alarm existence/state
semantic color
logical routing target
operational lifecycle/priority
```

Web does not read WAL directly.

## 10. Analytics

Analytics consumes durable facts such as:

```text
Occurrence/Episode
Journey
Evidence
management/deactivation
routing
priority transitions
configuration revisions
delivery/publication revisions when needed
```

Keep separate:

```text
Live Projection
Management Projection
History/Analytics
```

## 11. Known inverted dependencies

CURRENT implementation contains backend → Web dependencies, especially in Materialization acquisition.

These edges are **TO REMOVE / INVERT**, not contracts to preserve during the move.

## 12. domain/alarms

`scopes/ada-command-center/domain/alarms` currently contains residual source/routing policy.

Its final owner is OPEN.

The next design must classify each element:

```text
KEEP in Command Center
MOVE to Engine
REHOME to ada-contracts-alarms
REMOVE
```

## 13. Next design deliverable

Before code movement, produce:

```text
current dependency graph
target dependency graph
package MOVE/STAY/REMOVE/INVERT/REHOME matrix
exact publication boundary
incremental extraction order
qualification plan
```

Only after consensus should `scopes/ada-alarm-engine` be implemented.
