# ADA Manager — Datos operacionales: catálogo y asignaciones

Estado: **CURRENT DOMAIN / TOOL-SCOPE CUTOVER DECIDED / USER-RUNTIME SNAPSHOT PLANNED**

## Current domain contract

```text
AREA        mina | planta
GROUP       1 | 2 | 3 | 4
POSITION    stable id + editable label + active
ASSIGNMENT  user_id + area_id? + position_id? + group_id?
```

Assignment fields are optional. `None` is valid.

Operational attributes remain owned by `ada.web.operational.identification`, not by Atlanticus global Users identity.

## Current durable model

Operational uses Source + Projection:

```text
catalog Source
user assignment Source per user
→ projection
→ Cosmos consumption
```

Source history remains durable.

Operational assignment references a promoted `user_id`.

## Ownership refinement — DECIDED / NOT YET IMPLEMENTED

For ADA, Operational is Tool-specific.

Target durable path belongs under:

```text
StorageNamespace.scope_prefix
=
<ADA_APPLICATION_NAMESPACE>/<ADA_TOOL_NAMESPACE>
```

It must no longer be composed through the application-global Source root.

The only application-global Users data retained by the target model is identity.

## users-runtime integration — DECIDED TARGET

The Tool runtime user snapshot will contain resolved Operational values so login/runtime does not perform additional joins.

Stable shape:

```json
{
  "operational": {
    "area": {"id": null, "label": null},
    "position": {"id": null, "label": null},
    "group": {"id": null, "label": null}
  }
}
```

When data exists, IDs and labels are populated.

Operational Source remains the durable authority for Tool-specific operational assignment/catalog data. The denormalized runtime snapshot is the read authority for session consumption.

## Recovery

Immediate planned recovery uses a Tool Users Recovery Snapshot capable of restoring `users-runtime`.

FUTURE / PLANNED, explicitly not next:

```text
Global Users
+ Tool Membership
+ Profiles
+ Operational
→ join por IDs
→ users-runtime
```

## Access separation

Operational assignment does not grant ADA Access or Manager permissions.

Profile membership and Access remain separate contracts.

## Implementation rule

When this cutover is authorized:

```text
move Operational Source composition to tool_source
preserve current operational domain semantics
no compatibility application-scope Operational path
no adapter reading old and new paths
```

## OPEN

```text
Tool-scoped Operational storage cutover
Tool Users Runtime projection/snapshot composition
UI/session consumption of the new runtime snapshot
```

Do not reopen Operational catalog UX or Alarm/KPI work inside this increment.
