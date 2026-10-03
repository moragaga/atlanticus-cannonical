# Alarm Engine — Modeler and Delivery Pipeline

Estado: **TARGET DESIGN FROZEN PARTIAL / IMPLEMENTATION PLANNED**

Este documento congela la nueva responsabilidad Modeler y los límites de backpressure/recovery acordados durante el diseño de extracción Alarm Engine.

No declara implementación.

## 1. Why Modeler exists

Runtime owns operational truth.

Delivery should publish already-modeled documents.

Between them there is stateful logic that cannot live in Web and should not live in Delivery:

```text
ordering
logical positions
visible/hidden queues
rotation windows
carousel behavior
queue-in-queue behavior
timers
reconciliation after abrupt changes
recovery after restart
disconnection/staleness modeling
```

Therefore:

```text
Runtime
    ↓
Modeler
    ↓
Delivery
```

## 2. Configuration plane

Target Materialization:

```text
AlarmConfigurationSnapshot
        ↓
Materialization
        ├── RuntimeConfiguration
        ├── ModelerConfiguration
        └── DeliveryConfiguration
```

All three correspond to the same exact artifact pin.

The current `DeliveryAlarmConfiguration` is a mixed contract and must be split before Modeler implementation.

## 3. Data plane

Target:

```text
Runtime
    ↓ durable ordered input
Modeler
    ↓ durable modeled head per destination
Delivery
    ↓
Projection Store / Cosmos
    ↓
Web
```

History/Analytics continues to consume durable operational facts separately.

## 4. Runtime → Modeler invariants

FROZEN:

```text
Runtime never waits for Modeler.
Modeler owns its input checkpoint.
Input required for modeling is durable.
Required changes are processed in order.
Modeler consumes bounded batches / bounded memory.
Crash/restart cannot require Runtime replay from process memory.
```

### Exact source contract — OPEN

CURRENT implementation offers:

```text
CURRENT v1
FACTS v2
```

The next design must choose:

```text
coordinated consumption of CURRENT+FACTS
or
new coherent model-input document/batch
```

without temporal/artifact races.

## 5. Modeler internal state model — direction FROZEN

Conceptually separate:

### Candidate state

Operational alarm eligibility/state derived from Runtime.

### Scheduler state

Persistent information required to continue scheduling fairly and deterministically enough after restart.

Examples:

```text
queue membership
scheduler cursor
visible_since
timer anchor/deadline
last served candidate/component when needed
projection-mode partition state
input checkpoint
```

Do not persist data that can be safely re-derived unless it is needed for recovery/continuity.

### Projection head

Current logical output consumed by Delivery.

The projection head is not the authoritative Alarm lifecycle state; it is a modeled read state.

## 6. Reconciliation principle — FROZEN

Modeler is a reconciler, not a rigid list of imperative moves.

Input:

```text
previous scheduler state
+ new Runtime state/deltas
+ ModelerConfiguration
+ clock
```

Output:

```text
new scheduler state
+ current logical projection head
```

Operational truth wins over timer completion.

If an alarm:

```text
disappears
is managed
becomes suppressed/ineligible
moves out of target scope
```

it can leave the modeled visible set immediately.

The scheduler must tolerate:

```text
abrupt removals
many alarms becoming active together
many alarms disappearing together
restarts
late consumers
```

## 7. Timers — FROZEN behavior, exact duration OPEN

Timers belong to Modeler, not Runtime or Web.

Modeler may need to rotate even when Runtime emits no new Alarm change.

Exact duration remains OPEN:

```text
candidate range discussed: 90..120 seconds
```

No hardcoded value until decision.

## 8. CAROUSEL — DESIGN FROZEN partial

There are always six physical/logical visible positions for this modeled projection.

### 8.1 Zero or one DISTRIBUTED alarm

Use one carousel with six positions:

```text
1 2 3 4 5 6
```

A single DISTRIBUTED alarm participates in the same six-position carousel as GENERIC alarms.

Visible entries are compacted from left to right.

Additional eligible alarms wait.

When the left-most/current rotation candidate completes its window and remains eligible, it yields its position and returns to the waiting order according to the scheduler policy; the visible set compacts and the timer for its next exposure resets.

### 8.2 Two or more DISTRIBUTED alarms

The same six positions are partitioned logically:

```text
positions 1..5
    normal carousel

position 6
    DISTRIBUTED carousel
```

The normal carousel and DISTRIBUTED carousel have independent waiting queues and independent rotation timers.

Example:

```text
positions
1=A 2=B 3=C 4=D 5=E 6=DX

normal waiting
F G

distributed waiting
DY DZ
```

After normal rotation:

```text
1=B 2=C 3=D 4=E 5=F 6=DX
normal waiting: G A
distributed waiting: DY DZ
```

After distributed rotation:

```text
1=B 2=C 3=D 4=E 5=F 6=DY
normal waiting: G A
distributed waiting: DZ DX
```

Position 6 remains reserved while there are at least two eligible DISTRIBUTED alarms, even if positions 1..5 are not full.

### 8.3 Return from 2+ to 0..1 DISTRIBUTED

When eligible DISTRIBUTED count falls below two, return to the single six-position carousel by reconciliation.

Do not blindly reset all timers/state if valid continuity can be preserved.

Exact preservation policy will be specified with ModelerState/adoption rules.

## 9. QUEUE_IN_QUEUE — DESIGN FROZEN partial

Current operational topology described for Integrated Operations:

```text
MINE
    4 components
    3 visible alarm positions total

PLANT
    5 components
    3 visible alarm positions total

global visible maximum = 6
```

MINE and PLANT have independent scheduling flows.

A component may have multiple eligible alarms while only part of them are visible; therefore hidden candidates exist behind component-level selection.

Conceptual structure:

```text
MINE scheduler
    component queue/state A
    component queue/state B
    component queue/state C
    component queue/state D
        ↓
    3 visible slots

PLANT scheduler
    5 component queues/states
        ↓
    3 visible slots
```

### Fairness — OPEN

The exact algorithm still must define interaction between:

```text
rotation among hidden alarms of the same component
rotation toward alarms from other components not currently represented
new component activation
empty component removal
simultaneous changes
```

Do not implement a generic round-robin by inference.

## 10. Batch changes / initialization — FROZEN principle

When Runtime presents many changes together, Modeler should apply them as one logical reconciliation before publishing a new modeled revision.

Avoid:

```text
alarm A enters -> publish
alarm B enters -> publish
alarm C enters -> publish
...
```

when all belong to the same coherent input batch/cycle.

Prefer:

```text
apply complete batch
reconcile
persist
publish one current modeled revision
```

## 11. Recovery — DESIGN FROZEN

Modeler persists enough durable state to resume quickly.

Conceptual recovery:

```text
load ModelerState @ input checkpoint N
read input N+1..head in bounded batches
apply/reconcile
evaluate overdue timers against now
persist present state
publish present modeled head
continue
```

If downtime spans several rotation windows, do not emit every missed visible frame merely to catch up.

Preserve trace/history separately if contractually required.

## 12. Disconnection / stale behavior — responsibility FROZEN, policy OPEN

Modeler is responsible for representing/reconciling downstream logical state when Runtime input becomes stale/disconnected because this behavior requires clock + previous state.

Still OPEN:

```text
liveness source
threshold
exact visible behavior
recovery behavior after reconnection
```

Do not invent heartbeat frequency or timeout.

## 13. Modeler → Delivery — DESIGN FROZEN

Modeler leaves a durable current modeled head per destination/projection.

Delivery maintains independent progress per destination.

Semantics:

```text
latest-wins
```

If Modeler advances:

```text
41 -> 42 -> 43 -> 44 -> 45
```

while Delivery is unavailable, Delivery may publish current revision `45` directly when it recovers, provided the transport/read-model contract does not require intermediate revisions.

Historical transition facts may be retained separately.

One slow destination must not block other destinations.

## 14. Delivery target

Delivery target owns:

```text
destination/connection resolution already materialized
bounded concurrency
retry
transport
publication
physical store interaction
```

Delivery does not own:

```text
carousel
queue-in-queue
positions
dwell timers
fairness
model reconciliation
```

## 15. Web target

Web reads the modeled projection and renders it.

It does not reproduce Modeler scheduling.

Web owns visual technology:

```text
Dash
CSS
pixel geometry
interaction rendering
```

Logical slot identity/order comes from the modeled state.

## 16. Configuration adoption — OPEN

Need explicit policy for:

```text
Modeler state using artifact A
Runtime switches to artifact B
```

Must define:

```text
compatible state preservation
candidate reclassification
queue/timer preservation or reset
invalid target removal
checkpoint/artifact transition
```

No mixed-artifact modeled head is allowed.

## 17. Frozen invariants

```text
Runtime never waits Modeler.
Modeler never waits Delivery for its own durable commit.
Runtime -> Modeler is ordered/durable/no-drop for required changes.
Modeler -> Delivery is latest-wins per destination.
Runtime/Modeler/Delivery use the same exact artifact.
Modeler is backend logic, not Web.
Delivery transports; it does not model.
Web renders; it does not schedule.
No adapters/mirror Tool types to preserve old coupling.
```

## 18. Next implementation boundary

Before implementing this scheduler:

```text
split current DeliveryAlarmConfiguration
into exact ModelerConfiguration + DeliveryConfiguration
```

Then qualify that contract split independently.
