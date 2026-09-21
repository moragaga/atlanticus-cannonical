# Web Platform — Current Gaps

Estado: **CURRENT**

Checkpoint:

```text
moragaga/atlanticus@21cfb2f11362c1606ad14ff8adc7551948eced6a
```

## Closed in current storage/Tool persistence front

```text
ADA-STORAGE-NAMESPACE
CLOSED / VERIFIED / CURRENT

TOOL-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

TOOL-PERSISTENCE-RESILIENT-COMPOSITION
CLOSED / VERIFIED / CURRENT
```

Current Tool provider axes:

```text
Source      local | blob
Projection  local | cosmos
```

Current Tool resolution:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

## ADA Generic bootstrap gap

Current Generic Application still has:

```text
_StartupToolProjectionStore
resolve_current_tool_projection
```

and absence of current Tool Source raises `RuntimeError`.

It does not yet bind:

```text
environment/.env
AdaStorageNamespace
ToolPersistenceSettings
ToolPersistenceComposition
resolve_active_tool_projection
```

Gap:

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

## Collector runtime wiring

Collector capability itself is closed.

Its final operational wiring remains:

```text
PLANNED / AFTER ADA-GENERIC-OPERATIONAL-BOOTSTRAP
```

Do not bypass bootstrap by rebuilding Tool Projection directly from Source in the application.

## Users global namespace

Logical ownership is frozen:

```text
users = application-global
```

`AdaStorageNamespace.application_prefix` supports global path derivation.

Concrete Users store wiring to that namespace was not changed in this hito:

```text
UNVERIFIED / SEPARATE
```

## Manager provider wiring

Configuration Manager already uses generic Source/Projection contracts.

Its existing concrete local runtime was not migrated by this hito to
`ada-web-tools-persistence`.

```text
NOT PART OF NEXT FOCUS
```

## Python metadata

Project baseline:

```text
Python 3.14.7
```

Multiple packages, including new packages in this hito, still declare:

```text
requires-python ==3.14.2
```

Classification:

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / SEPARATE
```

Do not mix with ADA Generic bootstrap.

## Qualification transversal

```text
CI remote
UNVERIFIED

full Ruff workspace
UNVERIFIED

full monorepo pytest
UNVERIFIED
```
