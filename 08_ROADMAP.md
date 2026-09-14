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
WEB-STORAGE-TOPOLOGY                       CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY                     CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE            CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER               CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY          CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1                   CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2               CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER             CLOSED / VERIFIED / CURRENT
PROFILES-DOMAIN-EXTRACTION                 CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS                CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION                  CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT             CLOSED / VERIFIED / CURRENT
ADMIN-COMPOSITION-BACKEND                  CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY              CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-COMPOSITION           IN PROGRESS
```

Checkpoint actual:

```text
moragaga/atlanticus@9342769a626c39d1f7f860f81e051e2ef1300620
```

## Cierre backend de Admin Composition

Queda congelado:
- authoring backend canónico usa `UsersProfilesConfiguration`;
- no se crea aggregate admin mixto nuevo;
- `UsersProfilesAdminDraft` conserva exact `SourceSnapshot`;
- draft revision es local y no release identity;
- parser canónico no acepta shape legacy;
- Administrator explícito/no eliminable;
- Profile edit preserva key;
- delete referenciado requiere replacement;
- reassign+delete es transformación única;
- Managed creation parte de Pending;
- Managed identity no es editable;
- Users admin publication usa exact snapshot + CAS + basis release.

## Cierre Manager Exact-Source Boundary

Queda congelado:
- `ExactSourcePublicationWorkflow` es opt-in;
- no reemplaza `ConfigurationLifecycleWorkflow`;
- `get_source_snapshot()` devuelve `SourceSnapshot`;
- `publish_draft_exact(...)` recibe el snapshot esperado;
- `ExactSourcePublicationResult` conserva `PublishResult`;
- coordinator compara snapshot completo;
- los campos legacy textuales no se reinterpretan como release identity;
- no existe segundo coordinator.

## Qué permanece legacy

Continúan productivos hasta su cutover explícito:
- callbacks/layout/browser store administrativos de Users;
- `UsersConfigurationCatalog` en ese camino;
- `UsersAdministrationService` legacy y bundle/contracts asociados donde aún se consumen;
- Manager publication/verification/history textual para workflows legacy;
- runtime Managed writer;
- `projection_source_revision` en `users.runtime`.

## Siguiente foco aislado recomendado

```text
ADMIN-UI-DRAFT-CUTOVER  PLANNED / NEXT
```

Objetivo único:
- migrar callbacks, layout y store administrativo de Users Configuration al payload `UsersProfilesConfiguration`;
- usar `UsersProfilesAdminDraft` como contrato del draft nuevo;
- mantener el `SourceSnapshot` de base;
- aplicar las operaciones backend ya cerradas;
- retirar del camino UI activo la construcción/edición de `UsersConfigurationCatalog`.

No mezclar en este incremento:
- wiring Users↔Manager exact-source;
- runtime canonical cutover;
- runtime provenance;
- IndexedDB general del Manager;
- Navigation;
- Root physical config;
- Python migration;
- ADA Access;
- legacy deletion global.

## Incremento inmediatamente posterior

```text
USERS-MANAGER-EXACT-SOURCE-WIRING  PLANNED
```

Debe:
- implementar/adaptar el workflow productivo Users al protocolo `ExactSourcePublicationWorkflow`;
- transportar `SourceSnapshot` desde el admin workspace hasta publication;
- retornar `ExactSourcePublicationResult`;
- conservar autorización/audit/summary requeridos por Manager;
- no adaptar el snapshot exacto a `source_revision: str`.

## Después, como incrementos independientes

```text
USERS-RUNTIME-CANONICAL-CUTOVER           PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE    PLANNED
USERS-ADMIN-CANONICAL-MIGRATION           PLANNED
NAV-CONSUMER-MIGRATION-B                  PLANNED
DOMAIN-LEGACY-DELETION                     BLOCKED
```

El orden exacto después del wiring debe recalcularse sobre `main`.

## Runtime canonical cutover

Permanece PLANNED.

Debe materializar `users.runtime` desde contrato/projection canónico.

No mezclarlo con UI admin cutover.

## Runtime exact-release provenance

Permanece PLANNED.

No introducir shim `SourceReleaseId <-> str`.

## Root physical configuration

```text
ROOT-PHYSICAL-CONFIGURATION  PLANNED / UNVERIFIED
```

No bloquear Users/Profiles con deployment wiring.

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

El siguiente foco puede migrar el draft store actual de Users sin convertirlo en la implementación global de IndexedDB Manager.

## Python/Trixie

Python 3.14.7 + Trixie permanece decidido y pendiente global.

No mezclarlo con el siguiente foco.

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
