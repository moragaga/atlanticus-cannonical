# Web Platform — Current Gaps

Estado: **CURRENT**

## 1. Users / Navigation

### Bien separado

Packages core/configuration son independientes.

### Gap

ADA Configuration Manager enlaza Navigation con Users directamente para construir profile options.

Acción futura:
extraer ese enlace hacia una composition/binding opcional.

## 2. User Activity

### Existe

- event model;
- route changes;
- active time;
- route aggregates;
- Cosmos adapter;
- Memory adapter;
- Identity binding.

### Gap

No hay page visit history ordenada.

## 3. TTL

El contrato canónico requiere 24 h.

Debe verificarse dónde se declara físicamente el `CosmosContainerSpec` de User Activity.

No asumir que el TTL está aplicado sólo porque el dominio lo requiere.

## 4. Cosmos provisioning / Web lifecycle

Existe `CosmosProvisioner` y soporta:
- create database;
- ensure containers;
- validate containers;
- partition key validation;
- TTL validation.

Además están implementados y verificados:

```text
WEB-STORAGE-TOPOLOGY
USERS-STORAGE-TOPOLOGY
STORAGE-PREFLIGHT-COSMOS-BRIDGE
```

Gap vigente:
- integrar resource preparation al lifecycle Web;
- congelar `ApplicationResourcePlan`;
- required/optional semantics;
- named connection resolution global;
- política local/cloud de database creation;
- readiness READY/DEGRADED/ERROR.

## 5. Users runtime durable / Source / Projection

Cerrado y verificado:

```text
COSMOS-USERS-RUNTIME-ADAPTER       CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY  CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1           CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2       CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION          CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT     CLOSED / VERIFIED / CURRENT
```

Checkpoint CURRENT:

```text
moragaga/atlanticus@05d6cbb5b81b762f7fc06fc96b7959bfb835a7e3
```

Users dispone de:
- runtime durable común `users.runtime`;
- `UsersRuntimeStore` + `PendingUsersReader` Cosmos;
- writer Managed snapshot-level con Pending→Resolved, retirement/re-add y CAS;
- `ProfilesConfiguration` Profiles-owned;
- `UsersConfiguration` Users-owned;
- `UsersProfilesConfiguration` como cross-contract;
- Source canónico con una exact release y dos resources;
- canonical Projection `ProjectionRecord[UsersProfilesConfiguration]`;
- Cosmos ProjectionStore schema `2` con read schema `1`;
- exact release provenance en Projection;
- create-only + ETag/CAS;
- retry same-target idempotente.

Gap vigente:
- admin authoring sigue usando aggregate/contracts legacy;
- `users.runtime` conserva provenance legacy `projection_source_revision`;
- runtime writer todavía no consume directamente la Projection canónica separada;
- no está congelado el resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`;
- no se pueden eliminar todavía los contratos/adapters legacy de Users.

Siguiente frontera:

```text
USERS-PROFILES-ADMIN-COMPOSITION  PLANNED / NEXT
```

## 6. Storage provisioning

No se ha cerrado parity equivalente a Cosmos provisioning para toda `connectivity/storage`.

Debe diseñarse únicamente si Source/Blob/bootstrap lo requiere.

## 7. Manager bypass

`is_local` full-access bypass continúa como open item donde aún corresponda.

No mezclarlo con Users/Profiles admin composition.

## 8. Pre-Manager page

La superficie final de bootstrap/login permanece pendiente según canonical Manager.

## 9. Projection planner

Manager tiene workflows de proyección por módulo, pero aún debe congelarse un orquestador de proyecciones múltiples basado en dependencias si una necesidad real lo exige.

UCS-1 no introduce un segundo coordinator ni una Projection independiente de Profiles.

## 10. Command Center

Debe aplicar el mismo modelo de:
- capability independence;
- resource bootstrap;
- readiness;
- projection orchestration;
- optional User Activity.
