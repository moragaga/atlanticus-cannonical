# Alarm Engine — Source Ledger

Estado: **AUDIT LEDGER / UPDATED 2026-09-23**

## Implementation checkpoints

```text
Pure B.2 resolver
9398786ae9af7c00de1bcca9d7a311fe9ef2155f

Command Center Tools domain
9b9600ae96c9153cf70d0fb401905963b8583c2f

Alarm Tool Dependency Manifest v3
d2a5e14822d3711e64668b8e70cfa15d7ddae2f0

CURRENT main
880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
```

`880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6` está dos commits por delante de `d2a5e14822d3711e64668b8e70cfa15d7ddae2f0`.
Esos commits posteriores afectan `operational-data`/tooling y no alteran este cierre Alarm.

## Canonical base inspected

```text
148b178df74ee3083681140f3bb7997a02435b80
```

## Historical decisions

```text
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Relevant:
- B.1 Alarm Definition frozen inventory;
- B.2 Projection Boundary;
- B.2 Publication/Materialization increments 1 y 2.

## CURRENT refinements

Historical only:
- SharePoint as general physical authority;
- confirmed Tool catalog projected to Command Center Cosmos;
- same Alarm revision re-resolved against later Tool revision;
- full B.2 validity identical to save validity.

CURRENT:
- Storage/Blob authority where migrated;
- consolidated Tool Catalog ends in Storage;
- Alarm source v3 freezes exact Tool evidence;
- `VALID_AT_SAVE != READY != EFFECTIVE`.

## Qualification evidence

```text
domain/tools                   8 passed
domain/alarms                 50 passed
web/alarms/configuration      35 passed
configuration-manager         11 passed

ruff check                    GREEN
ruff format --check           GREEN
```

## Central implementation paths

```text
scopes/ada-command-center/domain/tools/
scopes/ada-command-center/domain/alarms/.../snapshot.py
scopes/ada-command-center/web/alarms/configuration/.../tool_dependencies.py
scopes/ada-command-center/web/alarms/configuration/.../tool_references.py
scopes/ada-command-center/web/alarms/configuration/.../workspace.py
scopes/ada-command-center/web/alarms/configuration/.../workflows.py
scopes/ada-command-center/web/alarms/configuration/.../source_release.py
scopes/ada-command-center/web/alarms/configuration/.../web/callbacks.py
scopes/ada-command-center/web/application/ada-command-center-configuration-manager/.../local_runtime.py
```
