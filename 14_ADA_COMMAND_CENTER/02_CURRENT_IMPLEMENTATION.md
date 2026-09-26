# ADA Command Center — Current Implementation

Estado: **VERIFIED / UPDATED 2026-09-23**

Corte auditado:

```text
moragaga/atlanticus@880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
```

## Físicamente en main

```text
scopes/ada-command-center/
├── domain/
│   ├── alarms/
│   └── tools/
├── backend/
│   ├── alarms/
│   │   ├── core/
│   │   ├── materialization/
│   │   └── persistence/
│   ├── processes/
│   │   └── alarms-runtime/
│   └── tools/
│       └── catalog/
└── web/
    ├── alarms/
    │   └── configuration/
    └── application/
        └── ada-command-center-configuration-manager/
```

No existe todavía una implementación específica de Alarm Configuration Cosmos Projection ni:

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

## Estado

```text
Alarm Domain                               CURRENT / IMPLEMENTED
Command Center Tools Domain                CURRENT / IMPLEMENTED
Tool Catalog V1                            CURRENT / IMPLEMENTED
Alarm Tool Reference reader                CURRENT / IMPLEMENTED
Alarm Tool Dependency Manifest             CURRENT / IMPLEMENTED
Alarm Configuration Source schema v3       CURRENT / IMPLEMENTED
Alarm Configuration base Projection        CURRENT / IMPLEMENTED
validate/publish Tool freeze               CURRENT / IMPLEMENTED
Pure B.2 resolver                          CURRENT / IMPLEMENTED
Alarm Configuration Cosmos Projection      PLANNED / NEXT
B.2 Materialization Process                PLANNED / AFTER
Runtime Effective Head                     PLANNED
Alarm Live Delivery                        PLANNED
Management Projection                      PLANNED
```

## `domain/tools`

```text
ada-command-center-tools-domain==1.0.0
```

Contracts:

```text
ToolDependencyEntry
ToolDependencyManifest
```

Entry stores key, display name, source release id, kind and full `ToolStructure`.

## Alarm snapshot v3

```text
AlarmConfigurationSnapshot
    configuration
    tool_dependencies
```

`confirmed_tool_catalog_revision` is derived from `tool_dependencies.revision`.

## Source contract

```text
document_type = ada_command_center_alarm_configuration_release
schema_version = 3
```

No v2 compatibility reader.

## Workspace/validation CURRENT

Alarm-specific sidecar:

```text
_confirmed_tool_catalog_revision
```

Manager generic was not modified.

Save Draft pins current Tool revision.
Validation requires pinned revision still current and all defined Tool keys present.
Publish repeats the check and persists selected dependency evidence.

## Base Projection CURRENT

Projection builder decodes Source and preserves the persisted snapshot unchanged.

Local runtime uses an in-process ProjectionStore.

## Pure B.2 CURRENT

Pure resolver remains no-I/O and can consume `ToolDependencyManifest` structurally as exact Tool
catalog evidence.

## Qualification

```text
domain/tools                   8 passed
domain/alarms                 50 passed
web/alarms/configuration      35 passed
configuration-manager         11 passed
```

Lint/formatter gates GREEN.

## Technical conflict

```text
Project baseline: Python 3.14.7
Command Center packages: ==3.14.2
```

OPEN / SEPARATE.
