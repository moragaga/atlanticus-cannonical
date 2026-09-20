# Web Platform — Current Gaps

Estado: **CURRENT**

Checkpoint:

```text
moragaga/atlanticus@783d3578da52aeb5cf831999a7717dc8b79f2fb0
```

## 1. Users

CURRENT:

```text
users/core
users/blob
users/cosmos
users/activity
web/compositions/users-manager

UserRecord.profile_key
EffectiveUser.profile_key
UsersAdministrationService
Users Manager Web surface
```

Removed / forbidden:

```text
authority_key
users/configuration
users generic Projection
users-manager Source/Projection module
```

## 2. Users Administration surface

```text
USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT
```

Users entra a Manager mediante `ManagerEntry`.

No queda gap de superficie administrativa Users dentro de este frente.

## 3. Users directory discovery

```text
UsersDirectoryReader
CURRENT CONTRACT

concrete Entra/Graph provider
UNVERIFIED
```

## 4. Profiles

CURRENT:

```text
profiles/core
profiles/configuration
profiles/projection-local
profiles/projection-cosmos
Profiles Configuration Web surface
profiles-manager composition
```

No queda gap de editor/Web surface/Projection contract de Profiles.

## 5. Generic Web pagination

```text
GENERIC-WEB-PAGINATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Users Manager consume el contrato compartido 10/20.

## 6. ADA Access

CURRENT:

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration
scopes/ada/web/access/projection-local
scopes/ada/web/access/projection-cosmos

profile_key -> access_keys
Source schema 2
AdaAccessProjectionBuilder
exact dependency -> Profiles Projection
durable local/Cosmos ProjectionRecord
```

Closed:

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT
```

Gap siguiente:

```text
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST
```

No existe Web surface ADA Access CURRENT.

Además, CURRENT no contiene catálogo/definición independiente de access permissions.
El siguiente diseño debe resolver cómo:

```text
crear/definir accesos
asignarlos a Profiles
obtener un identificador estable
permitir que el desarrollador use manualmente ese identificador en funcionalidades Web
```

No se ha decidido todavía:

```text
nombre del modelo de definición
schema exacto
persistencia exacta del catálogo
route exacta
identificador exacto distinto de/igual a access_key
```

No inventarlos antes del diseño.

## 7. Navigation / Profiles integration

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

## 8. Manager authorization

Core CURRENT:

```text
ManagerAuthorizationPolicy.can_view
ManagerModule | ManagerEntry
```

## 9. navigation-manager consumer mismatch

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

## 10. Manager final administrative composition

Profiles y Users ya están integrados.

Permanece:

```text
MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED / AFTER ADA ACCESS
```

## 11. User Activity

Permanece gap histórico de page visit history ordenada según target documentado.

## 12. TTL

Contrato canónico requiere 24 h para User Activity; verificar recurso físico antes de
declarar aplicado.

## 13. Cosmos provisioning / Web lifecycle

Permanecen gaps de resource preparation/readiness/named connections según consumers reales.

## 14. ADA Access runtime

```text
PLANNED / SEPARATE
```

No mezclar con la UI/configuration Manager.

## 15. Test hygiene

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

## 16. Python metadata

Canonical:

```text
Python 3.14.7
```

Packages aún contienen metadata 3.14.2 en múltiples boundaries.

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## 17. CI / global lint

```text
CI remoto
UNVERIFIED

full Ruff workspace
UNVERIFIED
```
