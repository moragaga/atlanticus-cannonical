# Web Platform — Users / Profiles / Navigation / Manager Capability Boundary

Estado: **CURRENT — Users + Navigation refined contracts implemented**.

## Generic ownership

```text
Atlanticus Users       identity / membership / runtime / recovery
Atlanticus Profiles    profile definitions / catalog
Atlanticus Navigation  route structure / authorization
Atlanticus Manager     administrative shell / authorization
Product                composition
```

No hacer que una capability genérica dependa de ADA sólo porque ADA sea el consumidor más avanzado.

## Global Users CURRENT

```text
UserIdentity
    user_id
    issuer
    subject_id
    display_name
    email
```

No contiene Tool profile/enabled state.

Physical scope target:

```text
<application>/users/users.json.gz
```

## Tool Membership CURRENT

```text
ToolUserMembership
    user_id
    profile_key
    enabled
```

Physical scope target:

```text
<application>/<tool>/users/memberships.json.gz
```

## users-runtime CURRENT

```text
RuntimeUser
    identity
    enabled
    profile
    operational
```

Tool-owned session/read authority.

One users-runtime Cosmos belongs to one Tool boundary; no duplicar `application_key`/`tool_key` dentro de cada item sólo para routing.

## Profiles CURRENT

Profiles es capability genérica y su Projection puede componerse con stores local/Cosmos. Compartir un container físico con otras projections sólo cuando topology/ownership sean compatibles y exista razón operacional; no hacerlo por copia literal de ADA.

## Navigation CURRENT

Persisted link contract:

```text
access_mode = PUBLIC | RESTRICTED
allowed_profiles = (...)
```

Semantics:

```text
PUBLIC                 -> accesible sin grant de profile
RESTRICTED + []        -> restringido sin perfiles habilitados
RESTRICTED + [profiles] -> sólo perfiles listados
```

Manager UI edita el modo explícitamente.

System `root/local` no se convierten en grants ordinarios por convenience; el privilege override pertenece a principal/authorization composition.

## Manager separation

No usar Navigation visibility como Manager authorization.

No mapear automáticamente Access específico de un producto a permisos Manager genéricos.

## Consumer parity rule

Los productos deben consumir estas capabilities mediante composición explícita.

ADA sirve como referencia implementada del patrón actual, pero Command Center no debe copiar lógica específica de ADA; debe alcanzar el mismo nivel usando los contracts genéricos.

## Clean cutover

Cuando una API anterior es reemplazada, remover el wiring viejo. No crear hidden compatibility aliases.
