# ADA Command Center — Identity, Users, Profiles, Access, Navigation and Activity

Estado: **CURRENT DIRECTION / REFINED AFTER USERS-PROFILES, ADA ACCESS PERSISTENCE AND PROFILES MANAGER INTEGRATION**

## Identity

Producción usa Microsoft Entra ID mediante capability transversal Atlanticus.

No crear autenticación paralela.

## Entrada a la aplicación

La identidad autenticada puede entrar aunque todavía no exista un `UserRecord` promovido.

```text
valid authenticated identity + no promoted UserRecord
→ READY
→ deterministic user_id
→ no UsersRuntime EffectiveUser

promoted + enabled=True
→ READY
→ EffectiveUser available

promoted + enabled=False
→ USER_DISABLED
→ 403
```

## Capability graph CURRENT

```text
Profiles
   ├──> Users
   ├──> Navigation Configuration
   └──> ADA Access
```

Esto representa consumo de contracts generic, no fusión de ownership.

No establecer:

```text
Navigation -> Users
Navigation -> ADA Access
Atlanticus Profiles -> ADA Access
```

## Users

Users es generic Atlanticus.

CURRENT:

```text
UserRecord.profile_key
EffectiveUser.profile_key
UsersAdministrationService
```

Users posee:

```text
user -> profile_key
```

No contiene ADA-specific access keys ni Navigation configuration.

Managed users consumen `ProfileCatalog`.

`local` es runtime-only y no managed assignment.

SUPERSEDED / REMOVED:

```text
authority_key
basic|root authority mini-contract
```

## Profiles

Profiles es capability generic Atlanticus first-class.

CURRENT:

```text
ProfileDefinition
ProfileCatalog
ProfilesConfiguration
Profiles Source lifecycle
Profiles Projection
Profiles Configuration Web surface
Profiles Manager composition
Profiles integration in ADA Configuration Manager
```

No agregar permisos ADA al modelo generic Profiles.

## ADA Access

ADA Access es application-specific bajo:

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration
```

CURRENT:

```text
ProfileAccessGrant
EffectiveAdaAccess
AdaAccessConfiguration
AdaAccessSourceService
AdaAccessProjectionBuilder
durable ProjectionRecord serialization
local Projection persistence
Cosmos Projection persistence
```

Ownership:

```text
profile_key -> ADA access_keys
```

REMOVED:

```text
UserProfileAssignment
user_id -> profile_keys
```

ADA Access Projection depende exactamente de Profiles Projection y valida sus referencias
contra el `ProfileCatalog` de esa dependencia.

La persistencia durable conserva el `ProjectionTarget` y dependencies exactas.
No reconstruir provenance desde la Profiles Projection CURRENT tras restart.

La UI/composition administrativa de ADA Access sigue PLANNED.

## Navigation

Navigation es generic y permanece independiente de Users y ADA Access.

Durable:

```text
allowed_profiles = profile keys
```

Navigation Configuration consume `ProfileCatalog` desde Profiles core.

No persiste copias de `ProfileDefinition`.

### Runtime fallback pendiente

La materialización exacta del principal de Navigation para identidad autenticada no
promovida sigue separada.

No crear Global User ficticio ni dependencias Navigation -> Users/ADA Access.

## Manager authorization

Core CURRENT:

```text
ManagerAuthorizationPolicy.can_view(...)
```

Profiles Manager usa esta semántica.

Permanece un consumer standalone desalineado en `navigation-manager`.

No introducir alias.

## User Activity

User Activity sigue opcional e integrable.

No es requisito para Alarm Engine ni Analytics.

## Reglas congeladas

```text
Entra valid identity without promotion
ACCESS ALLOWED

promoted disabled User
ACCESS BLOCKED

Users -> profile_key
CURRENT

Users -> ADA-specific access state
FORBIDDEN

Navigation -> Users
FORBIDDEN

Navigation -> ADA Access
FORBIDDEN

Navigation Configuration -> Profiles core
CURRENT

ADA Access
APPLICATION-SPECIFIC

ADA Access user_id -> profile_keys
REMOVED

Profiles
GENERIC ATLANTICUS

Navigation durable profile references
PROFILE KEYS

ADAPTERS / SHIMS / ALIASES
FORBIDDEN
```

## Pendientes separados

```text
Users Administration Manager integration
PLANNED / NEXT MANAGER FRONT

ADA Access Configuration Manager integration
PLANNED / AFTER USERS

ADA Access runtime composition
PLANNED / SEPARATE

Navigation operational authorization alignment
PLANNED / SEPARATE
```
