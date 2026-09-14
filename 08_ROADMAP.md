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
WEB-STORAGE-TOPOLOGY                    CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY                  CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE         CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER            CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY       CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1                CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2            CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER          CLOSED / VERIFIED / CURRENT
PROFILES-DOMAIN-EXTRACTION              CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS             CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION               CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT          CLOSED / VERIFIED / CURRENT
```

Checkpoint actual:

```text
moragaga/atlanticus@05d6cbb5b81b762f7fc06fc96b7959bfb835a7e3
```

## Cierre UCS-1

Queda congelado:
- `ProfilesConfiguration` Profiles-owned;
- `UsersConfiguration` Users-owned;
- `UsersProfilesConfiguration` como composición cross-contract;
- Administrator explícito y obligatorio en el payload canónico;
- `guest`/`local` fuera de Profiles funcionales;
- todos los Users, incluso disabled, deben resolver `profile_key`;
- una exact Source release con dos resources Users/Profiles;
- read Users Source v1 / write v2;
- canonical Projection payload `UsersProfilesConfiguration`;
- Cosmos Projection read v1 / write v2;
- exact-release invariants preservadas;
- no se creó segundo Source/coordinator ni `profiles.runtime`.

Permanece legacy:
- aggregate `UsersConfigurationCatalog`;
- bundle/services/callbacks administrativos que aún no migraron;
- runtime Managed writer;
- provenance `projection_source_revision` en `users.runtime`.

## Siguiente foco aislado recomendado

```text
USERS-PROFILES-ADMIN-COMPOSITION  PLANNED / NEXT
```

Objetivo:
- adaptar la experiencia administrativa para authoring conjunto de Users y Profiles;
- consumir/producir los contratos separados sin volver a crear un aggregate de ownership mixto;
- definir cómo se editan Administrator y Profiles funcionales en la nueva forma;
- preservar la regla de no orphan references;
- mantener una sola publicación exact-release;
- no tocar runtime canonical cutover ni provenance salvo bloqueo contractual real.

Debe auditar específicamente:
- callbacks/layout/store administrativo de Users Configuration;
- `UsersAdministrationService`;
- `UsersConfigurationBundle` y contracts legacy;
- `FileUsersProjectionProfileCatalog`;
- representación de drafts/admin payload;
- delete/reassign de Profiles referenciados;
- publicación hacia `UsersSourceService.publish_configuration(...)`.

## Después, como incrementos independientes

```text
USERS-RUNTIME-CANONICAL-CUTOVER           PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE    PLANNED
USERS-ADMIN-CANONICAL-MIGRATION           PLANNED
NAV-CONSUMER-MIGRATION-B                  PLANNED
DOMAIN-LEGACY-DELETION                     BLOCKED
```

El orden exacto después de `USERS-PROFILES-ADMIN-COMPOSITION` debe recalcularse sobre `main`.

## Runtime canonical cutover

Permanece PLANNED.

Debe materializar `users.runtime` desde el contrato/projection canónico separado.

No mezclarlo con la composición administrativa salvo necesidad contractual inevitable.

## Runtime exact-release provenance

Permanece PLANNED.

No introducir shim `SourceReleaseId <-> str`.

El runtime actual conserva provenance legacy hasta el cutover explícito.

## Root physical configuration

```text
ROOT-PHYSICAL-CONFIGURATION  PLANNED / UNVERIFIED
```

No bloquear el foco Users/Profiles con:
- env var names;
- Key Vault schema;
- Entra claim mapping;
- deployment wiring.

## Local identities

```text
LOCAL-JOHN-JANE-RUNTIME-CONTRACT  PLANNED / UNVERIFIED
```

Sólo está congelado que no son Profiles funcionales.

## Administrative migrations

Users y Navigation administrative migration permanecen PLANNED.

Legacy deletion permanece BLOCKED hasta demostrar que no quedan consumidores productivos legacy.

## Manager browser workspace

IndexedDB + `dcc.Store(memory)` permanece PLANNED.

No mezclarlo con Users/Profiles admin composition.

## Python/Trixie

Python 3.14.7 + Trixie permanece decidido y pendiente global.

No mezclarlo con el siguiente foco.

## Resource readiness

El resource físico de `users.runtime` está congelado.

La inventory global y el resource topology del canonical Users Projection store continúan abiertos.

No inferir `profiles.runtime`.

## Backend / Frontend productization

Se mantiene la prioridad de cerrar el Golden Path hacia un artefacto usable.

No usar los pendientes de plataforma como excusa para expandir arquitectura fuera de un bloqueo real.

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
