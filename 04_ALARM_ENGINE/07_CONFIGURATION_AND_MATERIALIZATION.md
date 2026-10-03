# Alarm Engine — Configuration and Materialization

Estado: **CURRENT implementation + target boundary frozen at contract level; physical extraction PLANNED/NEXT DESIGN**.

Checkpoint:

```text
atlanticus@346e7ac7ba7c21eede8b524613a6adee7e839e55
```

## Published configuration CURRENT

Shared configuration lives in `ada-contracts-alarms`.

```text
AlarmConfigurationSnapshot
    configuration: AlarmConfiguration
    tool_dependencies: ToolDependencyManifest
```

`ToolDependencyManifest` lives in `ada-contracts-tools`.

Published snapshot preserves the exact Tool Catalog revision used by Command Center.

## Command Center semantic ownership CURRENT

Before publication, Command Center owns:

```text
authoring
business validation
Tool reference validation/resolution
routing validation
visual target validation
publication
```

A published snapshot means valid/materializable according to the current contract.

## Materialization target

```text
published AlarmConfigurationSnapshot
        ↓
deterministic materialization
        ├── RuntimeAlarmConfiguration
        └── DeliveryAlarmConfiguration
```

Materialization must not rediscover Tools or redo semantic resolution already closed upstream.

`ResolvedAlarmConfiguration` as an extra published stage remains SUPERSEDED.

## Current implementation debt

Physical implementation still lives under Command Center backend and Materialization retains dependencies from the previous acquisition model.

Known debt includes backend dependencies toward Web/projection packages.

These dependencies are not target contracts.

## Next extraction rule

The next design increment treats:

```text
backend/alarms/*
backend/processes/alarms-*
```

as a single candidate Engine boundary.

During extraction, classify dependencies:

```text
KEEP
MOVE
REMOVE
INVERT
REHOME
```

Do not preserve backend → Web edges with shims.

## Pin/adoption invariants — FROZEN

```text
READY != EFFECTIVE
exact artifact pin
source_key + result_id + manifest_sha256 + resolution_key
Runtime and Delivery use same exact artifact
no fallback to latest READY
```

## Engine publication schemas CURRENT

Authority:

```text
ada-contracts-alarms/ada/contracts/alarms/schemas
```

Do not restore historical copies under backend.

## Separate blocker

The Tool Catalog qualifier failure caused by `ada.web.tools.*` vs `ada.contracts.tools.*` is outside this materialization/extraction increment.
