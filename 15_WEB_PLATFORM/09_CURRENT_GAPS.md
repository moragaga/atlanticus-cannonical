# Web Platform — Current Gaps

Estado: **CURRENT**

Checkpoint de implementación:

```text
moragaga/atlanticus@6dd09a6f24370bbad8ae358b6d5d7c6ea9aeba4a
```

## 1. Global Users

### Cerrado

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
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

### Gap vigente

Datos persistidos reales todavía no fueron migrados/inspeccionados en este cierre.

```text
USERS-PERSISTED-DATA-CUTOVER
PLANNED / NEXT
```

## 2. Users persisted data

Target de código CURRENT:

```text
Blob registry
users/users.json.gz
atlanticus_users_registry schema 1

Cosmos promoted store
atlanticus_user schema 1
```

Gap:

- inventory de datos legacy/current en environment real;
- migración one-shot;
- preservation de datos Profiles todavía útiles;
- parity verification Blob/Cosmos;
- deletion criteria para legacy persisted data;
- repair path para registry yes / Cosmos no.

No crear old-schema runtime readers.

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

## 4. Users Administration surface

Core administration lifecycle existe.

Gap:

```text
USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED
```

Debe mostrar/promover/actualizar/reparar lifecycle de Users sin volver a Manager Source.

## 5. Profiles / Access

CURRENT:

```text
profiles/core
profiles/configuration
```

Gap:

- independent Profiles lifecycle completo;
- app-specific Global User association;
- Access ownership/contract;
- Profiles admin/UI independiente.

No resolver esos gaps añadiendo app state a `UserRecord`.

## 6. Navigation integration

Navigation configuration permanece CURRENT.

Gap:

- definir cómo consume effective Profile/Access output;
- evitar ownership directo de Global Users;
- validar composition real cuando Profiles/Access contract esté congelado.

La antigua cadena Users Source → Profiles Source → Navigation ya no es CURRENT.

## 7. User Activity

### Existe

- event model;
- route changes;
- active time;
- route aggregates;
- Cosmos adapter;
- Memory adapter;
- Identity binding.

### Gap

No hay page visit history ordenada según el target documentado.

## 8. TTL

El contrato canónico requiere 24 h para User Activity.

Debe verificarse dónde se declara físicamente el `CosmosContainerSpec` correspondiente.

No asumir TTL aplicado sólo porque el dominio lo requiere.

## 9. Cosmos provisioning / Web lifecycle

Existe `CosmosProvisioner` y contratos previos de provisioning.

Permanecen gaps fuera de Users root cutover:

- integrar resource preparation al lifecycle Web donde corresponda;
- required/optional semantics;
- named connection resolution global;
- readiness READY/DEGRADED/ERROR.

## 10. Local runtime

Jane/John local identities permanecen en Users core.

Gap:

```text
composition/runtime wiring exacto del selector local
UNVERIFIED
```

## 11. Storage provisioning

No se ha cerrado parity equivalente a Cosmos provisioning para toda
`connectivity/storage`.

Diseñar sólo cuando un consumer real lo exija.

## 12. Manager bypass

`is_local` full-access bypass continúa como open item donde aún corresponda.

No mezclarlo con Users persisted-data cutover.

## 13. Projection planner

Manager tiene workflows de proyección por módulo.

No introducir coordinator global sin necesidad real demostrada.

## 14. Test hygiene

```text
WEB-TEST-CONTRACT-CLEANUP
PLANNED / OPEN
```

No añadir tests nuevos de CSS visual, source tokens, imports, AST o estructura interna.

## 15. Python metadata

Canonical:

```text
Python 3.14.7
```

CURRENT `web/pyproject.toml`:

```text
requires-python = "==3.14.2"
```

Gap VERIFIED / separado.

## 16. CI / global lint

CI remoto no tiene evidence asociada al checkpoint CURRENT.

Full Ruff workspace final no se declara PASS.

No mezclar cleanup ajeno con Users persisted-data cutover.
