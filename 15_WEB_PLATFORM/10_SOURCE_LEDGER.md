# Web Platform — Source Ledger

Estado: **AUDIT LEDGER**

Corte:
`moragaga/atlanticus@685924322c9cc0d625d112e25297a407f7a46acb`

## Users

`web/capabilities/users/`

- `core`
- `configuration`
- `activity`

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
- `CosmosContainerSpec`.

Verificado:

- ensure database;
- ensure containers;
- validate containers;
- partition key mismatch;
- TTL mismatch.

## Storage

Inspeccionado tree de `connectivity/storage`.

No se encontró provisioner.

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
