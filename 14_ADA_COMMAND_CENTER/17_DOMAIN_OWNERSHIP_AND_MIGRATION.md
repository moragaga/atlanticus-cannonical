# ADA Command Center — Domain Ownership and Migration

Estado: **CURRENT / ALARM DOMAIN + TOOLS DOMAIN IMPLEMENTED**

## Alarm authored domain

```text
scopes/ada-command-center/domain/alarms
ada-command-center-alarms-domain==1.0.0
```

Owns Alarm authoring contracts and `AlarmConfigurationSnapshot`.

## Command Center Tools shared domain

```text
scopes/ada-command-center/domain/tools
ada-command-center-tools-domain==1.0.0
```

Owns:

```text
ToolDependencyEntry
ToolDependencyManifest
```

This transversal contract is shared by Alarm publication/history and backend Materialization.

## Dependency direction CURRENT

```text
ada-web-tools structural contracts
        ↓
domain/tools
        ↓
domain/alarms snapshot wrapper
```

`domain/alarms` is no longer dependency-free.

The previous canonical statement `dependencies = []` is SUPERSEDED.

## Deferred normalization

Current Tools structural authority still lives under `ada-web-tools`.

Project decision:

```text
DOMAIN/TOOLS NORMALIZATION
PLANNED / DEFERRED
```

Do not refactor during Alarm Cosmos Projection unless it becomes a blocker.

## Materialization owner

```text
scopes/ada-command-center/backend/alarms/materialization
```

Owns pure resolution contracts/resolver, not stores/acquisition/orchestration.

## Process owner — planned

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

Still absent.

## Source/projection ownership

Alarm Configuration Web owns:
- Source codec/workflow;
- workspace correlation;
- base projection builder.

Next boundary is durable Cosmos Projection storage for `AlarmConfigurationSnapshot`.

## No legacy

No aliases/shims for:
- source schema v2;
- prior snapshot shape;
- old duplicate authored models.
