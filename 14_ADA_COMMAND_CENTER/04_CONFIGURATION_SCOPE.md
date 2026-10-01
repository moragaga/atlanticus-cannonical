# ADA Command Center — Configuration Scope

Estado: **CURRENT — Alarm Configuration Snapshot Source v3 / Tool dependencies Rn/Cn / Manager workspace converged / Command Center administration composition 0.1.2 CURRENT; durable E2E and Resource Preparation remain UNVERIFIED/PLANNED.**

## Authority of this close

```text
Implementation CURRENT
moragaga/atlanticus:main@736ae9820878a5d8ec7fa7f922ce483be3d3e6b3

Administration composition commit
moragaga/atlanticus@2ccc2dffd792d55ae68aee3d64ed73eef408bbf8
```

Command Center administers Alarm Configuration by reusing `atlanticus.web.manager` without modifying generic Manager semantics.

## Aggregate and publication — CURRENT

```text
AlarmConfiguration
    rules
    messages

AlarmConfigurationSnapshot
    configuration: AlarmConfiguration
    tool_dependencies: ToolDependencyManifest
    schema_version: 3
```

Tool definitions are not embedded in editable Alarm configuration.

The workspace preserves sidecar `_confirmed_tool_catalog_revision`, not a member of `AlarmConfiguration`.

Source schema v2 is SUPERSEDED without a legacy decoder.

### Current flow

```text
Save Draft -> reads Confirmed Tool Catalog and pins Cn in workspace
Validate   -> checks configuration/current Cn and Tool references
Verify     -> Source concurrency under Manager
Publish    -> checks Cn again and freezes manifest Cn
```

Later Tool Catalog changes do not reinterpret immutable Rn/Cn snapshots.

```text
VALID_AT_SAVE != READY != EFFECTIVE
```

## Manager workspace convergence — CURRENT

Packages:

```text
ada-command-center-web-alarm-configuration==0.1.1
atlanticus-web-manager==0.3.19
```

`AlarmConfigurationManagerWorkspaceBinding` retains only its real domain rule:

```text
Confirmed Tool Catalog required
→ pin current catalog revision into payload
```

Generic workspace behavior belongs to:

```text
ManagerWorkspaceBinding
→ owner validation
→ SourceKey validation
→ SourceSnapshot base
→ workspace document parsing/serialization
→ payload copy/update
```

No compatibility shim is retained.

Historical qualification preserved:

```text
124 PASS
Ruff PASS
```

## C1 — Tool ownership CURRENT

`web/tools/catalog` builds/persists Confirmed Tool Catalog in Blob; `web/tools/discovery-cosmos` inspects named Tool Cosmos connections and confirms revisions; `web/tools/catalog-manager` owns UI/callbacks.

`backend/tools` remains SUPERSEDED.

Tool Catalog Manager:

```text
ada-command-center-web-tool-catalog-manager==0.1.1
atlanticus-web-manager==0.3.19
```

Historical local qualification:

```text
9 PASS
Ruff PASS
```

**Current limit:** local Manager provider still requires Storage for Tool Catalog. Tool Catalog local filesystem is not implemented.

## Command Center administration composition — CURRENT

Package:

```text
ada-command-center-configuration-manager==0.1.2
```

The package now composes:

```text
Administration
├── Users
├── Profiles
└── Navigation

Configuration
├── Tool Catalog
└── Alarm Configuration
```

Access keys:

```text
users.manage
profiles.manage
navigation.manage
alarms.manage
```

Navigation configuration SourceKey:

```text
navigation
```

Profiles options for Navigation are projected through neutral `NavigationProfileOption` values. Navigation does not import Profiles directly.

Local administration topology currently implemented:

```text
Profiles Source       LocalSourceStore
Navigation Source     LocalSourceStore
Profiles Projection   in-process
Navigation Projection in-process
Users Registry        in-process
Users Promoted        in-process
```

Do not infer durable resource names, containers or restart persistence for these stores.

Qualification of 0.1.2:

```text
30 PASS
Ruff check PASS
Ruff format check PASS
git diff --check PASS
```

## C2 — Source Key and topology CURRENT

Domain defines:

```text
ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'
```

Web creates the technical `SourceKey`; Materialization/Runtime/Delivery consume the same logical identity.

Physical Alarm Projection identity remains:

```text
logical_id        ada.command_center.alarms.configuration.projection
physical_name     alarm-configuration
partition_key     /partition_key
allowed_override  CONNECTION_REF
```

SUPERSEDED physical name:

```text
ada-command-center-alarm-configuration-projection
```

No alias or compatibility layer is retained.

## Storage/local namespace CURRENT

```text
application_namespace = conciencia_situacional
tool_namespace        = command-center
```

Storage/Source:

```text
conciencia_situacional/command-center/tool-catalog/current.json
conciencia_situacional/command-center/sources/alarm-configuration/...
```

Alarm Projection local:

```text
<base_root>/conciencia_situacional/command-center/projections/alarm-configuration/...
```

Alarm Cosmos:

```text
alarm-configuration
PK /partition_key
```

Do not generalize this identity to resources that have no frozen contract.

## Configuration Manager application — CURRENT / SEPARATE

```text
ada-command-center-configuration-manager==0.1.2
```

This is a standalone development/testing/qualification application.

It is not the product root and is not intended to be executed as a nested host by Generic.

Its composition/contracts are legitimately reused by the product application.

## Generic Application relationship — CURRENT

```text
ada-command-center-generic-application==0.1.0
→ depends on configuration-manager==0.1.2
→ reuses ConfigurationManagerDependencies
→ reuses build_configuration_manager_surface(...)
→ consumes administration.navigation_projection_store
```

This establishes one composition authority rather than duplicating Manager modules.

## Physical configuration still OPEN

- Materialization durable Cosmos account/database/credential must match the physical resource used by the Web host: UNVERIFIED.
- Blob container remains environment-supplied.
- Tool Cosmos connections remain multiple/named where applicable.
- `APPLICATION=ada-command-center` remains common among the three jobs.
- `VOLUMEN_PATH` remains operator-managed and absolute.
- Durable Users/Profiles/Navigation topology is not frozen.
- Production identity binding for Generic 0.1.0 is not implemented.
- Tool Catalog local filesystem is not implemented.

## Existing cross-product dependency to audit

Current configuration code still reuses packages under `scopes/ada`, including the storage namespace implementation and `ada-web-tools`.

This is implementation reality, not a new contract.

Do not remove, alias or relocate it in this documentation close. The next tooling/distribution audit must expose whether it prevents an independent Command Center artifact and then propose the smallest root fix if required.

## OPEN separate fronts

- Dual-product tooling/distribution: PLANNED / NEXT.
- Resource Preparation + startup gate: PLANNED / DEFERRED.
- C3: real GREEN producer/verifiers remain unresolved; current manual qualification mechanism stays authoritative.
- C5: technical evidence key/version and environment audit remain open.
- Docker/Azure: UNVERIFIED.
- UX/END_OF_SHIFT: separate.
- Manager product header/branding: PLANNED / DEFERRED.
