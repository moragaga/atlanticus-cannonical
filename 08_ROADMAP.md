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
WEB-STORAGE-TOPOLOGY                 CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY               CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE      CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER         CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY    CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1             CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2         CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER       CLOSED / VERIFIED / CURRENT
PROFILES-DOMAIN-EXTRACTION           CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS          CLOSED / VERIFIED / CURRENT
```

Checkpoint final del cierre Profiles:

```text
moragaga/atlanticus@3ca92d5579e499dd4ab6413fa6d91c9d296b13c2
```

## Cierre Profiles Baseline

Queda congelado:
- Profiles core explícito y sin special identities;
- Guest/Pending fuera de Profiles;
- Administrator como Profile funcional;
- Root en Identity/bootstrap;
- Local/John/Jane fuera de Profiles;
- Users WebModule sin ownership de ProfileCatalog.

PB-4 y PB-5 quedaron absorbidos por PB-2.

PB-6 eliminó el service key `atlanticus.web.users.profiles`.

## Siguiente foco aislado recomendado

```text
USERS-CONTRACT-SEPARATION  PLANNED / NEXT
```

Objetivo del próximo hito:
- separar ownership contractual/durable de Users y Profiles;
- definir contratos antes de consumidores;
- preservar Source/Projection exact-release vigente;
- no introducir un segundo coordinator;
- no introducir shims;
- no cambiar todavía runtime provenance ni migración administrativa salvo que compile contractualmente lo exija.

Debe auditar específicamente:
- `UsersConfigurationCatalog`;
- campos durable Administrator/Guest;
- payload canónico `ProjectionRecord[UsersConfigurationCatalog]`;
- consumers que requieren Profiles authoring/runtime;
- frontera de validación `user.profile_key`.

## Después, como incrementos independientes

```text
USERS-PROFILES-ADMIN-COMPOSITION          PLANNED
USERS-RUNTIME-CANONICAL-CUTOVER           PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE    PLANNED
USERS-ADMIN-CANONICAL-MIGRATION           PLANNED
NAV-CONSUMER-MIGRATION-B                  PLANNED
DOMAIN-LEGACY-DELETION                     BLOCKED
```

El orden exacto después de `USERS-CONTRACT-SEPARATION` debe recalcularse sobre `main`.

## Root physical configuration

```text
ROOT-PHYSICAL-CONFIGURATION  PLANNED / UNVERIFIED
```

No bloquear `USERS-CONTRACT-SEPARATION` con:
- env var names;
- Key Vault schema;
- Entra claim mapping;
- deployment wiring.

Esas decisiones pertenecen a un incremento separado de composición/configuración.

## Local identities

```text
LOCAL-JOHN-JANE-RUNTIME-CONTRACT  PLANNED / UNVERIFIED
```

Sólo está congelado que no son Profiles funcionales.

## Runtime exact-release provenance

Permanece PLANNED.

No es NEXT mientras `UsersConfigurationCatalog` siga combinando ownership que debe separarse.

No introducir shim `SourceReleaseId <-> str`.

## Administrative migrations

Users y Navigation administrative migration permanecen PLANNED.

Legacy deletion permanece BLOCKED hasta demostrar que no quedan consumidores productivos legacy.

## Manager browser workspace

IndexedDB + `dcc.Store(memory)` permanece PLANNED.

No mezclarlo con Users/Profiles contract separation.

## Python/Trixie

Python 3.14.7 + Trixie permanece decidido y pendiente global.

No mezclarlo con `USERS-CONTRACT-SEPARATION`.

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
