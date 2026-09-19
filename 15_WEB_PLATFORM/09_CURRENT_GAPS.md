# Web Platform — Current Gaps

Estado: **CURRENT**

Checkpoint:

```text
moragaga/atlanticus@9f12c41a23d69784c7c5b775a4093a94ac654d55
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

Gap:

```text
Profiles Configuration UI
PLANNED
```

## 5. ADA Access

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
OPEN / SEPARATE
```

## 6. Navigation / Profiles integration

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

No existe dependency Navigation -> Users/ADA Access/Profiles Configuration.

## 7. Manager authorization

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

No bypass de `is_local` o `administrator`.

## 8. navigation-manager consumer mismatch

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

## 9. Configuration UI composition recovery

CURRENT app surface incluye:

```text
Navigation
Tools
KPI Configuration
KPI Definition
```

No incluye Profiles/Users Administration/ADA Access UI.

El usuario reporta visualizaciones/composiciones transversales previas perdidas.
Su existencia histórica concreta permanece UNVERIFIED hasta inspeccionar evidencia.

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
PLANNED / NEXT
```

## 10. User Activity

Permanece gap histórico de page visit history ordenada según target documentado.

## 11. TTL

Contrato canónico requiere 24 h para User Activity.
Verificar `CosmosContainerSpec` físico antes de declarar aplicado.

## 12. Cosmos provisioning / Web lifecycle

Permanecen gaps de resource preparation/readiness/named connections según consumers reales.

## 13. Local runtime

Local selector wiring exacto fuera de Configuration Manager continúa UNVERIFIED.

No usar ese gap para justificar authority implícita.

## 14. Test hygiene

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

## 15. Python metadata

Canonical:

```text
Python 3.14.7
```

Packages CURRENT aún tienen metadata 3.14.2.

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## 16. CI / global lint

```text
CI remoto 9f12c41...
UNVERIFIED

full Ruff workspace
UNVERIFIED
```
