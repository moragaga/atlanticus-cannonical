# Web Platform — Current Gaps

Estado: **CURRENT**

Checkpoint:

```text
moragaga/atlanticus@a31fce11d26a7c0a554d82de1813a4311522919b
```

## 1. Users

CURRENT:

```text
users/core
users/blob
users/cosmos
users/activity

UserRecord.profile_key
EffectiveUser.profile_key
```

Removed:

```text
authority_key
users/configuration
users generic Projection
users-manager Source/Projection
```

## 2. Users Administration surface

`UsersAdministrationService` existe y consume `ProfileCatalog`.

Gap:

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

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

## 6. ADA Access

CURRENT:

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration

profile_key -> access_keys
Source schema 2
AdaAccessProjectionBuilder
exact dependency -> Profiles Projection
```

Next gap:

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
PLANNED / NEXT / DESIGN FIRST
```

Otros gaps separados:

```text
ADA Access Configuration UI
PLANNED

ADA Access runtime composition
PLANNED
```

## 7. Navigation / Profiles integration

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

## 8. Manager authorization

Core CURRENT:

```text
ManagerAuthorizationPolicy.can_view
```

## 9. navigation-manager consumer mismatch

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

## 10. Manager final administrative composition

Profiles Manager composition reusable existe.

La integración final de Profiles/Users/ADA Access en una aplicación administrativa
completa sigue abierta.

## 11. User Activity

Permanece gap histórico de page visit history ordenada según target documentado.

## 12. TTL

Contrato canónico requiere 24 h para User Activity; verificar recurso físico antes de
declarar aplicado.

## 13. Cosmos provisioning / Web lifecycle

Permanecen gaps de resource preparation/readiness/named connections según consumers reales.

## 14. Local runtime

Local selector wiring exacto fuera de las compositions actuales continúa separado.

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
