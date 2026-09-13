# Web Platform — Current Gaps

Estado: **VERIFIED / CANDIDATE**

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

Debe verificarse dónde se declara físicamente el `CosmosContainerSpec` de User Activity porque no apareció en el barrido actual.

No asumir que el TTL está aplicado sólo porque el dominio lo requiere.

## 4. Cosmos provisioning / Web lifecycle

Existe `CosmosProvisioner` y ya soporta:

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

Por tanto el gap ya no es la traducción del resource plan a Cosmos.

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
```

Users dispone de:
- runtime durable común `users.runtime`;
- `UsersRuntimeStore` + `PendingUsersReader` Cosmos;
- writer Managed snapshot-level con Pending→Resolved, retirement/re-add y CAS;
- Source canónico sobre Source Core;
- Projection canónica desde `SourceReleaseRef` exacta;
- `ProjectionRecord[UsersConfigurationCatalog]`;
- Cosmos ProjectionStore con provenance `source_release_id`;
- create-only + ETag/CAS;
- retry same-target idempotente.

Checkpoint de canonical Projection:

```text
moragaga/atlanticus@139ee93a118e51f66c3d585f00235f212a2475c1
```

Gap vigente:
- Manager productivo aún usa `source_revision: str`;
- `users.runtime` conserva provenance legacy `projection_source_revision`;
- `UsersManagerWorkflowAdapter` no ha migrado;
- no está congelado el resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`;
- no se pueden eliminar todavía los contratos/adapters legacy de Users.

Siguiente frontera bloqueante:

```text
MANAGER-ROOT-CANONICAL-CUTOVER
```

## 6. Storage provisioning

No se encontró equivalente a `CosmosProvisioner` en `connectivity/storage`.

Debe diseñarse únicamente si Source/Blob/bootstrap lo requiere.

## 7. Manager bypass

Actualmente `is_local` obtiene acceso completo en:

- Manager authorization;
- ADA Manager module helpers.

Debe retirarse.

## 8. Pre-Manager page

No existe todavía una superficie de bootstrap equivalente en la aplicación ADA Configuration Manager auditada.

## 9. Projection planner

Manager tiene workflows de proyección por módulo, pero aún debe congelarse un orquestador de proyecciones múltiples basado en dependencias.

## 10. Command Center

Debe aplicar el mismo modelo de:

- capability independence;
- resource bootstrap;
- readiness;
- projection orchestration;
- optional User Activity.
