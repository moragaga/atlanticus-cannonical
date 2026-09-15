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
ADMIN-UI-DRAFT-CUTOVER                        CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-COMPOSITION              IN PROGRESS
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER PLANNED / NEXT CANDIDATE
```

Checkpoint actual:

```text
moragaga/atlanticus@d23bff025ab899367a8da1178dde5ab50806fe47
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

## Cierre Admin UI Draft Cutover

Queda congelado desde `d23bff...`:
- active Users admin layout/callbacks son canónicos;
- editor payload = `UsersProfilesConfiguration`;
- browser/local draft = `UsersProfilesAdminDraft` schema 2;
- exact `SourceSnapshot` se conserva durante edición/save local;
- browser draft schema 1/incompatible se descarta y se reconstruye clean desde Source current;
- no existe migración browser schema 1→2;
- Administrator/Profile/User edits usan operaciones canónicas;
- Managed create parte de Pending;
- save local no publica Source;
- import file legacy es compatibilidad de entrada explícita y termina en payload canónico;
- `UsersAdminWebContext` recibe `UsersProfilesAdministrationService`;
- este hito no cambia el service registration productivo Manager Users;
- este hito no implementa IndexedDB global Manager.

## Qué permanece legacy

Continúan hasta cutover explícito:
- `UsersManagerWorkflowAdapter` productivo del host ADA;
- publication/verification/history textual de workflows legacy;
- `build_users_history_preview` Users legacy;
- contratos/servicios legacy donde aún existan consumidores;
- runtime Managed writer;
- `projection_source_revision` en `users.runtime`;
- archivos legacy Web no activos hasta cleanup global.

Ya no son legacy activos del editor Users:
- callbacks/layout exportados para edición;
- payload local de edición;
- browser draft store del editor Users.

## Refinamiento de roadmap

El antiguo nombre:

```text
USERS-MANAGER-EXACT-SOURCE-WIRING
```

permanece refinado en:

```text
USERS-MANAGER-EXACT-SOURCE-COMPOSITION
    CLOSED / VERIFIED / CURRENT

USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER
    PLANNED
```

La existencia del adapter exact-source no equivale al cutover productivo del host.

El UI cutover quedó cerrado como incremento separado y no implica publication exact-source productiva.

## Siguiente foco aislado recomendado

```text
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER
PLANNED / NEXT CANDIDATE
```

Objetivo de debate/diseño antes de implementar:
- re-auditar el service registration Users del host ADA;
- definir cómo coexistirá o se reemplazará el workflow lifecycle legacy requerido por Manager;
- conectar `UsersManagerExactSourceWorkflow` a la publication productiva;
- usar `ManagerProjectionCoordinator.publish_draft_exact(...)`;
- transportar exact `SourceSnapshot` desde el draft schema 2;
- no reinterpretar release/token como strings;
- conservar autorización/audit;
- no reintroducir adapter canónico→`UsersConfigurationCatalog`;
- verificar el wiring productivo real de `users_profiles_administration`;
- adjudicar el drift de lock/contratos ADA necesario para poder calificar el host sin ampliar silenciosamente el alcance.

No mezclar:
- runtime canonical cutover;
- runtime provenance;
- Root physical config;
- Python migration;
- Navigation migration;
- legacy deletion global;
- Manager IndexedDB global;
- Profile replacement UX salvo que resulte estrictamente necesaria para publication.

## Hallazgos que condicionan el siguiente foco

### ADA dependency lock drift

VERIFIED:

```text
atlanticus-web-manager
  lock   0.3.14
  source 0.3.15

atlanticus-web-users-configuration
  lock   0.1.6
  source 0.1.9
```

El frozen host no puede calificarse limpiamente contra los sources actuales sin resolver este drift.

Un overlay efímero permitió calificar la composición Users del hito, pero la suite ADA completa expuso adapters legacy incompatibles con Manager 0.3.15.

No convertir automáticamente este hallazgo en una migración global de todos los adapters dentro del próximo incremento; si el cutover Users requiere cambios transversales, marcar BLOCKED y separar frontera.

### Productive dependency construction

`ConfigurationManagerDependencies` ahora exige `users_profiles_administration`, pero no se identificó dentro de `atlanticus` un constructor productivo concreto.

Verificación/wiring físico permanece OPEN dentro del próximo análisis del host.

## Después, como incrementos independientes

```text
USERS-RUNTIME-CANONICAL-CUTOVER           PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE    PLANNED
USERS-ADMIN-CANONICAL-MIGRATION           PLANNED
NAV-CONSUMER-MIGRATION-B                  PLANNED
DOMAIN-LEGACY-DELETION                    BLOCKED
```

El orden exacto después del productive exact-source cutover debe recalcularse sobre `main`.

## Profile delete replacement UX

Backend contract CLOSED: delete de Profile referenciado exige replacement y reasignación atómica.

UI actual bloquea/no ejecuta delete cuando hay referencias.

La UX explícita para elegir replacement permanece PLANNED y no reabre el backend contract.

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

Users administrative migration permanece IN PROGRESS hasta cerrar publication productiva y consumidores legacy relevantes.

Navigation administrative migration permanece PLANNED.

Legacy deletion permanece BLOCKED hasta demostrar ausencia de consumidores productivos legacy.

## Manager browser workspace

IndexedDB + `dcc.Store(memory)` general permanece PLANNED.

El Users admin UI usa `dcc.Store(memory)` para su estado activo, pero esto no declara implementado el WORKSPACE IndexedDB global.

## Python/Trixie

Python 3.14.7 + Trixie permanece decidido y pendiente global.

Qualification de `d23bff...` se ejecutó con Python 3.14.2 porque 3.14.7 no estaba disponible localmente.

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
