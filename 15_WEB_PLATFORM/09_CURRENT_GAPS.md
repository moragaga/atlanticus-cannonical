# Web Platform — Current Gaps

Estado: **CURRENT**

Checkpoint:

```text
moragaga/atlanticus@fbef06a8a0a587571527d9ecf131c73c5fc5f01a
```

## 1. Global Users

CLOSED:

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
USERS-PERSISTED-DATA-CUTOVER
```

CURRENT:

```text
users/core
users/blob
users/cosmos
users/activity
```

No existen CURRENT:

```text
users/configuration
users/projection-cosmos
users-manager Source/Projection composition
pending write during login
```

## 2. Users Administration surface

`UsersAdministrationService` existe.

Gap:

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

Debe consumir el lifecycle de Users directamente.

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
ProfileDefinition
ProfileCatalog
ProfilesConfiguration
Profiles Source lifecycle
```

Siguiente frontera:

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT
PLANNED / NEXT
```

Gap UI posterior:

```text
PROFILES-CONFIGURATION-WEB-SURFACE
PLANNED
```

## 5. Generic Web pagination

CLOSED:

```text
GENERIC-WEB-PAGINATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

CURRENT:

```text
atlanticus.web.pagination
PageRequest
Page
paginate_items
10 | 20
```

No incluye UI, CSS, placeholders, filtros ni sorting.

El contrato `ada.web.configuration.pagination` fue removido.

## 6. ADA Access

CURRENT:

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration
```

Gap UI:

```text
ADA Access Configuration UI
PLANNED
```

Gap runtime separado:

```text
ADA Access runtime composition exacta
PLANNED / SEPARATE
```

## 7. Navigation / Profiles integration

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

No existe dependency Navigation -> Users/ADA Access/Profiles Configuration.

## 8. Manager authorization

```text
MANAGER-AUTHORIZATION-SEMANTICS-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

CURRENT:

```text
ManagerModule.access_key
ManagerAuthorizationPolicy.can_view
explicit principal.access_keys
```

## 9. navigation-manager consumer mismatch

Gap verificado:

```text
web/compositions/navigation-manager
calls can_access(...)

ManagerAuthorizationPolicy
exposes can_view(...)
```

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

## 10. Configuration UI composition recovery

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
CLOSED / VERIFIED / CURRENT
```

Resultado:

```text
shared behavior only with demonstrated reuse
presentation remains owned by each module
pagination extracted as generic behavior
```

No crear un shared admin UI framework por simetría visual.

## 11. User Activity

Permanece gap histórico de page visit history ordenada según target documentado.

## 12. TTL

Contrato canónico requiere 24 h para User Activity.
Verificar `CosmosContainerSpec` físico antes de declarar aplicado.

## 13. Cosmos provisioning / Web lifecycle

Permanecen gaps de resource preparation/readiness/named connections según consumers reales.

## 14. Local runtime

Local selector wiring exacto fuera de Configuration Manager continúa UNVERIFIED.

No usar ese gap para justificar authority implícita.

## 15. Test hygiene

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

Los I001 observados en `tests/test_web_runtime.py` de KPI Configuration/Definition son
preexistentes y no fueron absorbidos por pagination cutover.

## 16. Python metadata

Canonical:

```text
Python 3.14.7
```

Packages CURRENT aún tienen metadata 3.14.2.

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## 17. CI / global lint

```text
CI remoto fbef06a8...
UNVERIFIED

full Ruff workspace
UNVERIFIED
```
