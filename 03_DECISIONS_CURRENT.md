# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

```text
Python 3.14.7
uv, no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
no double contract
one focus per increment
```

## Storage namespace

CURRENT/FROZEN:

```text
physical container != application namespace != tool namespace != SourceKey
```

ADA:

```text
application namespace
conciencia_situacional

tool namespaces
operaciones_integradas
mina
...
```

Logical topology:

```text
<application>/
├── users/
└── <tool>/
    ├── sources/
    └── projections/
```

`users` is global.

`SourceStore` owns the `sources/` segment.

No `../` path reconstruction from Tool to global resources.

## Tool Projection persistence

CURRENT/FROZEN:

```text
ProjectionRecord[ToolConfiguration]
LocalToolProjectionStore
CosmosToolProjectionStore
```

Cosmos:

```text
partition_key = <application>/<tool>
SourceKey = tools
```

Namespace belongs to the store instance, not to domain Source identity.

## Provider composition

CURRENT/FROZEN:

```text
Source provider     local | blob
Projection provider local | cosmos
```

Independent valid combinations:

```text
local + local
blob + cosmos
blob + local
local + cosmos
```

CURRENT API:

```text
compose_tool_persistence
resolve_active_tool_projection
project_current_tool_source
```

Resolution states:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

## Runtime dependency rule

CURRENT/FROZEN:

```text
runtime active Tool read
→ Projection durable first
→ does not require Source availability
```

Source participates when a workflow needs to select/project the current Source release.

Do not rebuild active runtime configuration from Source on every application startup.

## Availability rule

CURRENT/FROZEN:

```text
APPLICATION EXISTENCE
!= TOOL CONFIGURATION EXISTENCE
!= EXTERNAL INFRASTRUCTURE AVAILABILITY
!= BUSINESS DATA AVAILABILITY
```

Valid non-fatal states include:

```text
no Source current
no active Tool Projection
no KPI Latest
no KPI Timeseries
provider temporarily unavailable
```

The affected capability must expose/degrade its state instead of turning absence into a global
Web startup failure.

Invalid contract remains visible and diagnostic.

## Collector decisions

Collector contracts remain CURRENT/FROZEN.

Do not reopen:

```text
polling intervals
one store per ToolComponent
Subcomponent boundary
Latest priority
browser cache-only behavior
compatibility semantics
```

## Decisions superseded/refined by this closure

```text
"next = attach Collector directly from Tool Source"
SUPERSEDED / REFINED
```

The missing prerequisite was the durable Tool Projection/provider composition and its availability
boundary.

```text
"in-process startup Tool Projection is the operational runtime route"
SUPERSEDED AS TARGET DIRECTION
```

It remains in current ADA Generic code only until the next bootstrap cutover.

```text
"Source must be available to resolve Tool runtime"
SUPERSEDED
```

Runtime may consume an existing durable Tool Projection independently of Source availability.

## Next decision boundary

No new provider architecture is required.

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

Implement existing contracts into the real ADA Generic startup/runtime.

Do not redesign namespace, Tool Projection, SourceStore, Projection Core or Collector.
