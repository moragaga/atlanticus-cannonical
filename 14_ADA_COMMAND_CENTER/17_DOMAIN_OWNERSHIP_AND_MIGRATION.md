# ADA Command Center — Domain Ownership and Migration

Estado: **CURRENT — shared contracts extracted; consumers cut over in main; qualification IN PROGRESS**.

Checkpoint:

```text
atlanticus@6725237a19c4442fdfa1b32c3410c124e9348dbc
```

## Shared Tool contracts — CURRENT

```text
scopes/ada-contracts/tools
ada-contracts-tools==1.0.0
```

Owns reusable Tool enums, structure/source contracts and dependency manifest.

`scopes/ada-command-center/domain/tools` está **SUPERSEDED / REMOVED**.

## Shared Alarm contracts — CURRENT

```text
scopes/ada-contracts/alarms
ada-contracts-alarms==1.0.0
```

Owns reusable Alarm configuration models/snapshot/errors and Engine publication schemas.

Depends on `ada-contracts-tools`.

## Command Center Alarm domain — CURRENT

```text
scopes/ada-command-center/domain/alarms
```

Owns only Command Center-specific cross-boundary policy:

```text
ALARM_CONFIGURATION_SOURCE_KEY
next_routing_tool_kind()
```

No compatibility re-exports of the moved shared models.

## Tool Web services — CURRENT

```text
web/tools/catalog
web/tools/discovery-cosmos
web/tools/catalog-manager
```

Tool Catalog stores neutral `ada.contracts.tools` models. Conversion from current ADA ToolConfiguration is explicit at the catalog boundary.

## Backend Engine — CURRENT physical location

```text
backend/alarms/core
backend/alarms/materialization
backend/alarms/persistence
backend/processes/alarms-materialization
backend/processes/alarms-runtime
backend/processes/alarms-delivery
```

El cutover no mueve todavía estos owners.

## Schema ownership — CURRENT

```text
ada-contracts-alarms/src/ada/contracts/alarms/schemas
```

Las copias históricas de `backend/alarms/contracts` están removidas.

## Target boundary — DECIDED / PLANNED implementation cleanup

```text
Command Center
  authoring + semantic validation + resolution + publication
        ↓
AlarmConfigurationSnapshot válido
        ↓
Materialization deterministic splitter
        ├── RuntimeAlarmConfiguration
        └── DeliveryAlarmConfiguration
```

Materialization CURRENT aún conserva debt de la frontera anterior. No crear shim; reemplazar limpiamente en un incremento posterior.

## Qualification CURRENT

El consumer cutover está implementado y pasó gates parciales a través de:

```text
contracts
domain alarms
core
materialization
persistence
delivery
runtime
Web Alarm Configuration
```

El qualifier queda **BLOCKED** en Command Center Configuration Manager por drift de generic Users/Profiles/Navigation/Manager.

## Physical extraction

`ada-alarm-engine` permanece **PLANNED**.

No mover código físicamente hasta cerrar qualification y dependency direction. Atlanticus core genérico no debe depender de ADA.

## No legacy

No restaurar `domain/tools`, no crear re-export packages, no mantener tipos duplicados y no esconder dependencias reales mediante pytest/source-path hacks.
