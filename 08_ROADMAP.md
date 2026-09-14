# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0**

## Regla

El bootstrap arquitectónico se considera cerrado para comenzar ejecución.

Los puntos aún no definidos quedan como `OPEN` y se resuelven cuando el primer Golden Path los necesite.

No continuar expandiendo arquitectura general antes de avanzar producto.

## PRIORIDAD 1 — Operaciones Integradas

Primera Tool real.

Cerrar verticalmente:

1. Tool Configuration;
2. Component contracts;
3. data/collector mapping;
4. KPI;
5. Alarm integration;
6. ADA Generic;
7. Manager;
8. artifact/distribution.

## PRIORIDAD 2 — Mina

Repetir el patrón estabilizado con Operaciones Integradas.

## Web Platform

En paralelo sólo los enablers que bloquean la vertical:

- Login/Bootstrap Console;
- base projections;
- derived resolutions;
- resource readiness;
- Manager authorization sin bypass.

Resource readiness / Users durable + Source chain tiene el siguiente checkpoint:

```text
WEB-STORAGE-TOPOLOGY              CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY            CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE   CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER      CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1          CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2      CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER    CLOSED / VERIFIED / CURRENT
```

Las fronteras permanecen separadas:
- Storage Topology declara y resuelve recursos;
- el bridge Cosmos traduce/prepara topology provider-specific;
- `CosmosUsersRuntimeStore` implementa lectura/observación;
- `CosmosUsersRuntimeProjectionWriter` materializa snapshots Managed en `users.runtime` dentro del camino legacy;
- `UsersSourceService` conecta Users con Source Core sin reimplementar releases, History ni CAS;
- `UsersProjectionBuilder` + `SourceProjectionService` conectan una `SourceReleaseRef` exacta con `ProjectionRecord[UsersConfigurationCatalog]`;
- `CosmosUsersConfigurationProjectionStore` persiste la Projection canónica con create-only + ETag/CAS;
- Manager root transporta `ProjectionTarget` exacto en la acción Project sin degradarlo a `source_revision: str`.

Checkpoints relevantes:

```text
USERS-CANONICAL-PROJECTION-2
moragaga/atlanticus@139ee93a118e51f66c3d585f00235f212a2475c1

MANAGER-ROOT-CANONICAL-CUTOVER
moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
```

El cierre de Manager se limita al root productivo de la acción Projection. Publicación/verificación/history administrativa y browser WORKSPACE conservan contratos separados que siguen pendientes de migración cuando corresponda.

Siguiente foco aislado recomendado:

```text
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE  PLANNED
```

Objetivo:
- reemplazar el provenance Managed basado en `projection_source_revision` por identidad Source exact-release;
- conservar el ownership snapshot-level y las invariantes de `users.runtime` ya cerradas;
- no equiparar `UsersConfigurationBundle.revision` con `SourceReleaseId`;
- no introducir shim `SourceReleaseId <-> str`.

No mezclar este foco con:
- migración administrativa Users;
- migración administrativa Navigation;
- migración Python 3.14.7;
- lifecycle global de resource readiness;
- Profiles / ADA Access;
- cleanup general de legacy;
- browser IndexedDB del Manager;
- projection orchestration multi-capability.

Después, como incrementos independientes:
- migración administrativa Users: `PLANNED`;
- migración administrativa Navigation: `PLANNED`;
- legacy deletion de cada dominio: `BLOCKED` hasta validar consumidores migrados.

## Backend productization

Cerrar:

- `REPROCESS_CURRENT`;
- process generation;
- artifact/distribution contract;
- component scripts;
- master integrity check;
- env.detail;
- READMEs.

## Frontend productization

Cerrar:

- generated ADA Generic application;
- distributable artifact;
- loaders;
- readiness/bootstrap integration;
- alarm management consuming backend contract;
- ADA Component External Links.

## Command Center

Primero:

1. Web/configuration foundation;
2. Alarm B.2 integration;
3. History/Data Sufficiency qualification.

No implementar analytics todavía.

Después de data sufficiency GREEN:

```text
History read model
→ analytics
→ dashboard/storytelling
```

## Infrastructure inventory

No congelar la inventory global de containers hasta disponer de traza completa de recursos usados por Operaciones Integradas y las capabilities asociadas.

`users.runtime` sí queda confirmado individualmente y no implica que la inventory global esté cerrada.

El resource físico de `CosmosUsersConfigurationProjectionStore` no queda congelado por `USERS-CANONICAL-PROJECTION-2`; el provider recibe `container_name` desde composición.

## University

Crear casos pequeños a medida que una capability queda estable.

No esperar al final para escribirlos todos.

## Delivery boundary

Atlanticus produce:

```text
validated source
→ artifact
→ distribution-ready package
```

No es owner del pipeline corporativo DevOps.
