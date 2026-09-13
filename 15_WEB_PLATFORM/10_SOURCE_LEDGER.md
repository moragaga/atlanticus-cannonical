# Web Platform — Source Ledger

Estado: **AUDIT LEDGER**

Corte:
`moragaga/atlanticus@d63886d8d42688e3d03d680f0a3d7b92cd3863ed`

## Users

`web/capabilities/users/`

- `core`
- `configuration`
- `activity`
- `cosmos`

Verificado en el corte actual:
- `UsersRuntimeStore` expone `resolve/observe`;
- `PendingUsersReader` expone `list_pending`;
- `users.runtime` es el único recurso durable Users confirmado;
- Pending y Managed comparten el mismo recurso;
- `CosmosUsersRuntimeStore` implementa ambos contratos;
- `resolve()` usa point-read;
- `observe()` usa create-only + conflict reread;
- `list_pending()` usa query cross-partition y orden determinista;
- Users Configuration mantiene `UsersProjectionRepository.load_state/project/health_check`;
- permanece sin cerrar el writer durable Managed desde Projection hacia `users.runtime`.

## Navigation

`web/capabilities/navigation/`

- `core`
- `configuration`

## Optional composition precedent

`web/compositions/navigation-activity/`

Integra:

- Navigation;
- User Activity;

sin acoplar los packages core.

## User Activity

Inspeccionado:

- `models.py`
- `services.py`
- Cosmos adapter.

Verificado:

- session summary;
- route aggregates;
- route change events;
- Identity usage;
- falta page visit history ordenada.

## Cosmos

Inspeccionado:

- `CosmosProvisioner`;
- `CosmosContainerSpec`;
- Web Storage Topology;
- Web Cosmos Storage Bridge.

Verificado:

- ensure database;
- ensure containers;
- validate containers;
- partition key mismatch;
- TTL mismatch;
- `ResolvedStoragePlan` → `CosmosContainerSpec`;
- múltiples provisioners nombrados por `connection_ref`;
- bridge sin DB creation, secrets, raw settings ni Azure SDK;
- errores locales detectables antes del provider I/O.

## Storage

Inspeccionado tree de `connectivity/storage`.

No se encontró provisioner equivalente a `CosmosProvisioner`.

## Manager

Inspeccionado:

- `authorization.py`;
- ADA Manager composition;
- application;
- `/manager` page.

Verificado:

- bypass `principal.is_local`;
- Navigation profile options leídas desde Users en composición ADA;
- no existe pre-Manager bootstrap page.

## Qualification incorporada al ledger

`STORAGE-PREFLIGHT-COSMOS-BRIDGE`:
- 17 focalizados;
- 51 con Storage Topology;
- 87 con Users core;
- 448 passed / 7 skipped Web en su checkpoint.

`COSMOS-USERS-RUNTIME-ADAPTER`:
- 23 focalizados;
- 110 combinados;
- 471 passed / 7 skipped Web final;
- Ruff/format/lock/public API/`git diff --check` GREEN.
