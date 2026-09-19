# Web Platform — Current Gaps

Estado: **CURRENT**

Checkpoint de implementación:

```text
moragaga/atlanticus@3eb46dac80f23d438774e3afa39999dc96f592d7
```

## 1. Global Users

Cerrado:

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

USERS-PERSISTED-DATA-CUTOVER
CLOSED / VERIFIED / CURRENT
```

CURRENT:

```text
users/core
users/blob
users/cosmos
users/activity
```

Eliminado:

```text
users/configuration
users/projection-cosmos
compositions/users-manager
combined Users/Profiles configuration lifecycle
pending write during login
```

Managed authority CURRENT:

```text
basic
root
```

Runtime local:

```text
local
```

No existe `guest` ni `administrator` como Users authority CURRENT.

## 2. Users Administration surface

Core administration lifecycle existe.

Gap:

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED / SEPARATE
```

Debe consumir el lifecycle de Users directamente y no reconstruir Users Source/Projection.

## 3. Users directory discovery

Boundary CURRENT:

```text
UsersDirectoryReader
```

Gap:

```text
concrete Entra/Graph provider
UNVERIFIED
```

## 4. Profiles

Cerrado:

```text
PROFILES-CAPABILITY-EXTRACTION
CLOSED / VERIFIED / CURRENT

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
CLOSED / VERIFIED / CURRENT
```

CURRENT:

```text
profiles/core
profiles/configuration
ProfileDefinition
ProfileCatalog
ProfilesConfiguration
Profiles Source lifecycle
```

No agregar estado app-specific a Profiles generic.

## 5. ADA Access

Cerrado:

```text
ADA-ACCESS-PROFILES-CONFIGURATION
CLOSED / VERIFIED / CURRENT
```

ADA Access permanece application-specific.

Gap separado:

```text
runtime composition exacta de ADA Access
OPEN / SEPARATE
```

No convertirla en dependencia de Navigation.

## 6. Navigation / Profiles integration

Cerrado:

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

CURRENT:

```text
Navigation Configuration -> Profiles core
NavigationProfileCatalogProvider
ProfileCatalog / ProfileDefinition
allowed_profiles = durable profile keys
shared draft/projection validation
```

Eliminado:

```text
NavigationProfileOption
_BASE_PROFILES
NavigationProfileOptionsProvider
profile_options_provider
local/administrator/guest mini-catalog
```

No existe dependencia de Navigation hacia Users, ADA Access o Profiles Configuration.

Gap separado:

```text
exact guest fallback composition for authenticated non-promoted identity
OPEN / SEPARATE
```

## 7. Manager authorization

`DefaultManagerAuthorizationPolicy` CURRENT todavía permite full access mediante:

```text
principal.is_local
OR
'administrator' in principal.profile_keys
```

ADA Configuration Manager repite semántica equivalente en helpers `_can_manage_*`.

Gap:

```text
Manager authorization stale administrator/local semantics
OPEN / PROPOSED NEXT
```

No asumir la solución antes de revisar runtime local y consumers.

## 8. User Activity

Existe:

- event model;
- route changes;
- active time;
- route aggregates;
- Cosmos adapter;
- Memory adapter;
- Identity binding.

Gap:

No hay page visit history ordenada según el target documentado.

## 9. TTL

El contrato canónico requiere 24 h para User Activity.

Debe verificarse dónde se declara físicamente el `CosmosContainerSpec` correspondiente.

No asumir TTL aplicado sólo porque el dominio lo requiere.

## 10. Cosmos provisioning / Web lifecycle

Existe `CosmosProvisioner` y contratos previos de provisioning.

Permanecen gaps:

- integrar resource preparation al lifecycle Web donde corresponda;
- required/optional semantics;
- named connection resolution global;
- readiness READY/DEGRADED/ERROR.

## 11. Local runtime

Jane/John local identities permanecen en Users core.

Gap:

```text
composition/runtime wiring exacto del selector local
UNVERIFIED
```

No usar este gap para justificar `administrator` implícito.

## 12. Storage provisioning

No se ha cerrado parity equivalente a Cosmos provisioning para toda
`connectivity/storage`.

Diseñar sólo cuando un consumer real lo exija.

## 13. Projection planner

Manager tiene workflows de proyección por módulo.

No introducir coordinator global sin necesidad real demostrada.

## 14. Test hygiene

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

Durante el último hito quedaron findings fuera de scope en:

```text
capabilities/navigation/configuration/tests/test_web_contract.py
capabilities/navigation/configuration/tests/test_web_source_contract.py
```

No añadir tests nuevos de CSS visual, source tokens, imports, AST o estructura interna.

## 15. Python metadata

Canonical:

```text
Python 3.14.7
```

Navigation Configuration CURRENT:

```text
requires-python = "==3.14.2"
```

Gap:

```text
PYTHON-METADATA-ALIGNMENT
PLANNED / OPEN
```

## 16. CI / global lint

CI remoto para `3eb46dac...` no fue verificado en este cierre.

Full Ruff workspace tampoco se declara PASS.

```text
UNVERIFIED
```
