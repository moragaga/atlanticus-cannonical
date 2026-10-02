# ADA Web — Current Baseline

Estado: **CURRENT — UI CAPABILITIES AVAILABLE; DATA E2E STILL OPEN**

## Application

ADA Generic is the product composition root.

The distribution/runtime ownership cleanup does not move ADA UI capabilities into tooling.

## Content State

Current domain states:

```text
READY
STALE
SOURCE_ERROR
CONSTRUCTION
```

Freshness mapping:

```text
FRESH       → READY
PREVENTIVE  → READY
HARD_STALE  → STALE
DATA_ERROR  → SOURCE_ERROR
```

No explicit `NO_DATA` or `WAITING` state exists.

## Presentation modes

```text
NORMAL
AUTHORING
```

`AUTHORING` suppresses degraded overlay visibility while preserving the real content state.

This provides a presentation mechanism for UI authoring, but does **not** prove that every component can render without data.

## Current UI risk before Collector E2E

OPEN / UNVERIFIED:

```text
application starts without KPI observations
→ component-level behavior across all UI surfaces
```

Do not equate:

```text
no first observation
=
source failure
```

until the Collector/UI integration is exercised.

## Planned sequence

```text
env.detail contracts
→ dual application smoke
→ ADA KPI/Collector E2E
→ UI reconstruction
```

UI design can reuse existing capabilities/components; avoid redesigning generic contracts until a concrete consumer finding requires it.
