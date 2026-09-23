# ADA Command Center — Source Ledger

Estado: **AUDIT LEDGER / UPDATED 2026-09-23**

## Current implementation

```text
moragaga/atlanticus@880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
```

Relevant closure commits:

```text
9b9600ae96c9153cf70d0fb401905963b8583c2f
-> Command Center domain/tools + ToolDependencyManifest

d2a5e14822d3711e64668b8e70cfa15d7ddae2f0
-> AlarmConfigurationSnapshot v3
-> Tool manifest persistence
-> workspace Tool revision pin
-> validation/publication drift guard
-> local runtime integration
```

Commits after `d2a5e14822d3711e64668b8e70cfa15d7ddae2f0` up to `880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6` belong to an unrelated operational-data front.

## Current canonical base inspected

```text
moragaga/atlanticus-cannonical@148b178df74ee3083681140f3bb7997a02435b80
```

## Historical decisions

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## Qualification observed

```text
domain/tools
8 passed

domain/alarms
50 passed

web/alarms/configuration
35 passed

configuration-manager
11 passed
```

Lint and format gates GREEN.

## Contract changes

### Added

```text
scopes/ada-command-center/domain/tools
ToolDependencyEntry
ToolDependencyManifest
```

### Alarm Source

Previous:

```text
AlarmConfigurationSnapshot
    configuration
    confirmed_tool_catalog_revision
```

CURRENT:

```text
AlarmConfigurationSnapshot
    configuration
    tool_dependencies
```

`confirmed_tool_catalog_revision` is derived.

```text
v2 -> SUPERSEDED
v3 -> CURRENT
```

No legacy v2 decoder.

### Workspace

Added:

```text
_confirmed_tool_catalog_revision
```

Manager generic unchanged.

### Tool reader

One Tool snapshot read now produces authoring references + dependency evidence.

## Historical conflicts

`atlanticus-decisions` remains useful history but is stale in:
- SharePoint physical authority;
- re-resolution against later Tool revision without Alarm republish;
- consolidated Tool output to Command Center Cosmos.
