# Users — Global Identity, Tool Runtime Snapshot and Recovery

Estado: **CURRENT / IMPLEMENTED en capabilities; consumer parity parcial**.

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

## Materialization CURRENT contract

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

Es superficie runtime/read de una Tool. No usarla como reemplazo del store administrativo de Membership.

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

## Consumer state

ADA consume este modelo CURRENT.

Command Center todavía usa wiring anterior en `main@6725237...`; su paridad es **PLANNED / NEXT** y debe reemplazar la API vieja sin adapters.

## Future

Continuous/event-driven materialization puede evaluarse después si existe requisito real. No es prerequisito para corregir la composición actual de Command Center.
