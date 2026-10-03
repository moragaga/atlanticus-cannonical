# Web Platform — Users / Profiles / Navigation / Manager Capability Boundary

Estado: **CURRENT — generic contracts implemented; ADA and Command Center consumer parity CURRENT**.

## Generic ownership

```text
Atlanticus Users       identity / membership / runtime / recovery
Atlanticus Profiles    profile definitions / catalog
Atlanticus Navigation  route structure / authorization
Atlanticus Manager     administrative shell / authorization
Product                composition
```

Una capability genérica no depende de ADA ni Command Center.

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

Physical scope:

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

Physical scope:

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

Es autoridad runtime/read Tool-scoped.

## Profiles CURRENT

Profiles es capability genérica y su Projection se compone con stores local/Cosmos.

## Navigation CURRENT

Persisted link contract:

```text
access_mode = PUBLIC | RESTRICTED
allowed_profiles = (...)
```

Semantics:

```text
PUBLIC                  -> accesible sin grant de profile
RESTRICTED + []         -> restringido sin perfiles habilitados
RESTRICTED + [profiles] -> perfiles listados
```

Root/local privilege override pertenece a principal/authorization composition y no se persiste como grant ordinario.

## Manager separation

```text
Navigation visibility != Manager authorization
Product access != generic Manager access
```

## Consumer parity CURRENT

ADA y Command Center consumen estas capabilities mediante composición explícita.

Command Center CURRENT incluye:

```text
UsersAdministrationService(registry, memberships, profiles, directory)
generic Profiles manager
generic Navigation manager
generic NAVIGATION_SOURCE_KEY
UsersRuntimeStore
ManagerPrincipalBinding
Users special recovery integration
```

## Clean cutover

Las APIs superseded no deben permanecer como aliases ocultos.

No restaurar:

```text
UserRecord
UsersAdministrationStore
users_promoted
promoted=
local duplicate navigation source authority
```
