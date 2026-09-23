# ADA Command Center — Tool Catalog

Estado: **CURRENT V1 / STORAGE OUTPUT CLOSED / ALARM DEPENDENCY FREEZE INTEGRATED**

## Backend owner

```text
scopes/ada-command-center/backend/tools/catalog
```

## Contract

```text
ToolCatalogEntry
    tool_key
    display_name
    kind
    source_release_id
    structure

ToolCatalogSnapshot
    revision
    generated_at_utc
    tools
```

Revision is deterministic from normalized Tool entries.

## Consolidation

The package consumes configured Tool ProjectionStore inputs and produces an all-or-nothing snapshot.

A failed refresh does not overwrite current Storage state.

## Durable output

```text
ToolCatalogStore
BlobToolCatalogStore
```

V1 persists one CURRENT catalog in Storage/Blob.

No catalog history or retention is implemented here.

## Command Center topology

Operational integration may observe multiple upstream Tool Cosmos/projection surfaces plus prior
Storage state during reconciliation/certification.

Confirmed output:

```text
Confirmed Tool Catalog -> Storage
```

It intentionally does not project back into Command Center Cosmos.

## Consumer — Alarm Configuration

One exact snapshot produces:

```text
AlarmToolReferenceCatalog
    catalog_revision
    UI tools
    dependencies: ToolDependencyManifest
```

UI tools omit STRATEGIC.
Dependency catalog preserves all snapshot entries.

## Shared downstream Tool contract

Owner:

```text
scopes/ada-command-center/domain/tools
```

```text
ToolDependencyEntry
ToolDependencyManifest
```

Shared by Alarm publication/history and backend Materialization.

## History strategy

Tool Catalog Store still keeps only CURRENT.

Historical exact Tool evidence required by an Alarm revision is persisted in:

```text
AlarmConfigurationSnapshot.tool_dependencies
```

Therefore B.2 does not require versioned historical lookup from `BlobToolCatalogStore`.

## Authoring behavior

`AlarmConfiguration` remains Rules + Messages.

But Alarm Manager publication is no longer externally unrestricted:
- Save Draft requires current Confirmed Tool Catalog;
- validation requires referenced keys in pinned revision;
- publish rejects revision drift.

This refines the old "catalog is only optional authoring assistance" statement.

## Non-goals

- Tool authoring in Command Center;
- consolidated Tool Cosmos output;
- B.2 inside Tool Catalog;
- Tool catalog history solely for Alarm materialization.
