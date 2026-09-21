# Web Platform — Open Items

Estado: **PLANNED OPEN ITEMS**

Los items cerrados no deben reabrirse para restaurar simetría, legacy o contratos transitorios.

## Closed baseline relevant to next focus

```text
ADA-STORAGE-NAMESPACE
CLOSED / VERIFIED / CURRENT

TOOL-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

TOOL-PERSISTENCE-RESILIENT-COMPOSITION
CLOSED / VERIFIED / CURRENT

ADA-WEB-KPI-COLLECTOR-CAPABILITY
CLOSED / VERIFIED / CURRENT
```

## Next authorized frontier

Un único foco:

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

Debe inspeccionar y modificar la composición/runtime existente, no crear otra aplicación.

Required chain:

```text
Web environment/settings
→ provider/client settings
→ AdaStorageNamespace
→ ToolPersistenceComposition
→ resolve_active_tool_projection
→ existing ADA Generic runtime/composition
```

Acceptance direction:

```text
READY        → Tool Projection usable
UNCONFIGURED → base Web still runs
UNAVAILABLE  → base Web still runs; capability degraded
INVALID      → base Web remains diagnosable; no silent fallback
```

## Collector after bootstrap

Only after the Tool bootstrap boundary is real:

```text
Tool Projection READY
→ ToolStructure
→ Collector
→ Latest/Timeseries
```

Collector contract must not be redesigned.

## Separate open items

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / SEPARATE

CI remote
UNVERIFIED

full Ruff workspace
UNVERIFIED

full monorepo pytest
UNVERIFIED
```

Do not mix these with the next chat.

## Forbidden shortcuts

```text
no legacy
no adapter/shim
no double contract
no hardcoded Tool configuration
no in-process Projection as durable authority
no Source-required runtime read
no fallback to another provider when configured provider is unavailable
```
