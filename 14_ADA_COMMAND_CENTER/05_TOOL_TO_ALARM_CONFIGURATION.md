# ADA Command Center — Tool to Alarm Configuration

Estado: **CURRENT / CONFIRMED TOOL STORAGE + EXACT ALARM DEPENDENCY FREEZE IMPLEMENTED**

## Ownership

Tool configuration owns:
- `tool_key`;
- display name;
- kind;
- Components/Subcomponents;
- topology and relationships.

Alarm Configuration stores references plus frozen evidence in its versioned snapshot.

## Command Center Tools topology

```text
Tool A projection/Cosmos ─┐
Tool B projection/Cosmos ─┼─> reconciliation/certification
Tool C projection/Cosmos ─┘          +
                              prior Storage state
                                     |
                                     v
                          Confirmed Tool Catalog Cn
                                     |
                                     v
                                  Storage
                                     |
                                    END
```

Do not create:

```text
Confirmed Tool Catalog -> Command Center Cosmos
```

## Tool Catalog CURRENT

Backend:

```text
scopes/ada-command-center/backend/tools/catalog
```

Entry:

```text
tool_key
display_name
kind
source_release_id
structure
```

Snapshot:
- deterministic revision;
- unique/sorted keys;
- one CURRENT Blob snapshot;
- overwrite-on-success;
- no Command Center Cosmos output.

## One read, two consumers

`AlarmToolReferenceReader.load()` reads one snapshot and derives:

```text
UI reference catalog
+
full ToolDependencyManifest
```

UI excludes STRATEGIC suggestions.
Dependency evidence retains all snapshot entries.

## Alarm publication correlation

```text
current Tool Catalog = Cn
        |
        v
Save Draft pins Cn
        |
        v
Validate Cn
        |
        v
Publish checks Cn again
        |
        v
select referenced Tool entries
        |
        v
AlarmConfigurationSnapshot(Rn, Cn)
```

If current becomes `Cn+1` before publish, publish blocks.

## ToolDependencyManifest

Each selected Tool freezes:

```text
tool_key
display_name
source_release_id
kind
ToolStructure
```

`ToolStructure` preserves Component/Subcomponent names and topology.

## Superseded re-resolution model

Do not do:

```text
Alarm R1/C1
Tools advances C2
B.2 resolves R1 against C2
```

without a new Alarm publication.

CURRENT:

```text
R1 -> frozen C1 evidence
R2 -> may adopt C2
```

A new Tool revision alone does not invalidate existing Alarm revision.

## B.2

B.2 consumes the exact manifest carried by Alarm source/projection.

It does not access Tool Cosmos and does not need historical Tool Catalog lookup.

Tool GREEN qualification remains an explicit input; producer still OPEN.

## History rationale

`display_name` is intentionally frozen so historical alarm movement/routing can be explained with the
names/topology valid at publication time.
