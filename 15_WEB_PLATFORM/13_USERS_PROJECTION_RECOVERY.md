# Users — Global Identity, Tool Runtime Snapshot and Recovery

Estado: **CURRENT / IMPLEMENTED — ADA and Command Center consumers aligned**.

## Global identity CURRENT

```text
UserIdentity
```

Durable ownership:

```text
<application>/users/users.json.gz
```

## Tool Membership CURRENT

```text
ToolUserMembership
```

Durable ownership:

```text
<application>/<tool>/users/memberships.json.gz
```

Global Users y Tool Membership son autoridades distintas.

## Materialization CURRENT

```text
Global Users
+ Tool Membership
+ Profiles
+ Operational
→ RuntimeUser[]
```

RuntimeUser contiene identity, enabled, profile y operational.

## Cosmos users-runtime CURRENT

```text
item id       = user_id
partition key = user_id
```

Es superficie runtime/read de una Tool. No es el store administrativo de Membership.

## Recovery CURRENT

Physical snapshot scope:

```text
<application>/<tool>/users/recovery/snapshots
```

Operation:

```text
validate
before-image
audit started
replace_all
verify exact match
audit completed/failed
```

Complete REPLACE only.

Global Users y Membership no son mutados por recovery de runtime.

## Master Projection

Users sigue siendo operación especial snapshot/recovery, no un Source Projection domain ordinario.

`MasterProjectionExecutor` soporta `apply_users` cuando la composición inyecta `users_replace`.

Command Center durable composition puede inyectar:

```text
users_recovery
users_snapshot_ids
```

y habilitar esa operación.

## Consumer state

```text
ADA             CURRENT
Command Center  CURRENT at atlanticus@346e7ac7...
```

La API anterior de Users en Command Center queda SUPERSEDED.

## Future

Continuous/event-driven materialization sólo se evalúa ante requisito real. No es prerequisito del modelo actual.
