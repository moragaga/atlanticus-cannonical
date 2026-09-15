# Atlanticus — Roadmap

Estado: **CANONICAL BASELINE 1.0 — EXECUTION IN PROGRESS**

## Regla

No expandir arquitectura general sin necesidad de producto.

Cerrar verticalmente capacidades integrables y verificables.

Mantener un foco por incremento.

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

Repetir el patrón estabilizado.

## Web Platform — checkpoint actual

```text
WEB-STORAGE-TOPOLOGY                          CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY                        CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE               CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER                  CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY             CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1                      CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2                  CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER                CLOSED / VERIFIED / CURRENT
PROFILES-DOMAIN-EXTRACTION                    CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS                   CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION                     CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT                CLOSED / VERIFIED / CURRENT
ADMIN-COMPOSITION-BACKEND                     CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY                 CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-DRAFT-BASELINE-SEMANTICS CLOSED / VERIFIED / CURRENT
USERS-MANAGER-EXACT-SOURCE-COMPOSITION        CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-COMPOSITION              IN PROGRESS
```

Checkpoint actual:

```text
moragaga/atlanticus@7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98
```

## Cierre draft baseline semantics

Queda congelado:
- draft schema `2`;
- `base_payload_revision`;
- create = clean;
- `has_local_changes`;
- edit preserva BASE + SourceSnapshot;
- rebase adopta nuevo exact SourceSnapshot;
- schema 1 no se migra;
- local revision nunca es Source identity.

## Cierre Manager exact-source composition

Queda congelado:
- adapter vive en `web/compositions/users-manager`;
- composition depende de Manager + Source + Users Configuration;
- Manager no depende de Users;
- Users Configuration no depende de Manager;
- payload se revalida como `UsersProfilesConfiguration`;
- actor usa `UsersAuditActorProvider`;
- audit timestamp usa release publicada;
- no persiste/rebasa draft;
- no registra servicios del host;
- no proyecta.

## Qué permanece legacy

Continúan productivos hasta cutover explícito:
- callbacks/layout/browser store administrativos de Users;
- `UsersConfigurationCatalog` en ese camino;
- `UsersAdministrationService` legacy y bundle/contracts asociados donde aún se consumen;
- `UsersManagerWorkflowAdapter` del host ADA;
- Manager publication/verification/history textual para workflows legacy;
- runtime Managed writer;
- `projection_source_revision` en `users.runtime`.

## Refinamiento de roadmap

El antiguo nombre:

```text
USERS-MANAGER-EXACT-SOURCE-WIRING
```

queda refinado en:

```text
USERS-MANAGER-EXACT-SOURCE-COMPOSITION
    CLOSED / VERIFIED / CURRENT

USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER
    PLANNED
```

La existencia del adapter no equivale al cutover productivo del host.

## Siguiente foco aislado recomendado

```text
ADMIN-UI-DRAFT-CUTOVER  PLANNED / NEXT
```

Objetivo único:
- migrar callbacks, layout y store administrativo de Users Configuration al payload `UsersProfilesConfiguration`;
- usar `UsersProfilesAdminDraft` schema 2;
- preservar exact `SourceSnapshot`;
- usar `revision/base_payload_revision` para estado local;
- aplicar operaciones backend ya cerradas;
- retirar del camino UI activo la construcción/edición de `UsersConfigurationCatalog`.

No mezclar:
- runtime canonical cutover;
- runtime provenance;
- Root physical config;
- Python migration;
- Navigation migration;
- legacy deletion global;
- Manager IndexedDB global.

## Incremento posterior separado

```text
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER  PLANNED
```

Debe:
- reemplazar el service registration productivo legacy Users por la composition exact-source;
- usar el payload canónico ya migrado;
- transportar exact `SourceSnapshot`;
- usar `ManagerProjectionCoordinator.publish_draft_exact(...)`;
- no reinterpretar release/token como strings;
- mantener autorización/audit;
- no reintroducir adapter hacia `UsersConfigurationCatalog`.

## Después, como incrementos independientes

```text
USERS-RUNTIME-CANONICAL-CUTOVER           PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE    PLANNED
USERS-ADMIN-CANONICAL-MIGRATION           PLANNED
NAV-CONSUMER-MIGRATION-B                  PLANNED
DOMAIN-LEGACY-DELETION                     BLOCKED
```

El orden exacto después del productive exact-source cutover debe recalcularse sobre `main`.

## Runtime canonical cutover

Permanece PLANNED.

Debe materializar `users.runtime` desde contrato/projection canónico.

## Runtime exact-release provenance

Permanece PLANNED.

No introducir shim `SourceReleaseId <-> str`.

## Root physical configuration

```text
ROOT-PHYSICAL-CONFIGURATION  PLANNED / UNVERIFIED
```

## Local identities

```text
LOCAL-JOHN-JANE-RUNTIME-CONTRACT  PLANNED / UNVERIFIED
```

Sólo está congelado que no son Profiles funcionales.

## Administrative migrations

Users y Navigation administrative migration permanecen PLANNED.

Legacy deletion permanece BLOCKED hasta demostrar ausencia de consumidores productivos legacy.

## Manager browser workspace

IndexedDB + `dcc.Store(memory)` general permanece PLANNED.

El UI draft cutover de Users no debe declararse implementación global de IndexedDB Manager.

## Python/Trixie

Python 3.14.7 + Trixie permanece decidido y pendiente global.

El workspace y el nuevo package `users-manager` todavía conservan `requires-python ==3.14.2`.

No mezclar esta migración con el siguiente foco.

## Resource readiness

El resource físico de `users.runtime` está congelado.

La inventory global y resource topology del canonical Users Projection store continúan abiertos.

No inferir `profiles.runtime`.

## Backend / Frontend productization

Se mantiene la prioridad de cerrar el Golden Path hacia artefacto usable.

## Command Center

Mantener orden:
1. Web/configuration foundation;
2. Alarm B.2 integration;
3. History/Data Sufficiency qualification;
4. analytics sólo después de GREEN.

## University

Crear casos pedagógicos pequeños cuando una capability se estabilice.

## Delivery boundary

Atlanticus produce:

```text
validated source
→ artifact
→ distribution-ready package
```

No es owner del pipeline corporativo DevOps.
