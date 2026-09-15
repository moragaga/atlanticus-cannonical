# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades.

No reinterpretar un FAIL histórico como fallo vigente sin revisar su adjudicación.

## Checkpoints principales preservados

| Hito | Estado | Evidencia resumida |
|---|---|---|
| WEB-STORAGE-TOPOLOGY / USERS-STORAGE-TOPOLOGY | CLOSED / VERIFIED | focal + Web suite GREEN |
| STORAGE-PREFLIGHT-COSMOS-BRIDGE | CLOSED / VERIFIED | bridge + Storage + Users GREEN |
| COSMOS-USERS-RUNTIME-ADAPTER | CLOSED / VERIFIED | package + integrated Web GREEN |
| USERS-RUNTIME-PROJECTION-BOUNDARY | CLOSED / VERIFIED | focal + Web GREEN |
| USERS-CANONICAL-SOURCE-1 | CLOSED / VERIFIED | Source Users + Web GREEN |
| USERS-CANONICAL-PROJECTION-2 | CLOSED / VERIFIED | exact-release + Cosmos Projection GREEN |
| MANAGER-ROOT-CANONICAL-CUTOVER | CLOSED / VERIFIED | Manager + Web GREEN |
| PROFILES-DOMAIN-EXTRACTION | CLOSED / VERIFIED | Profiles + Users + Web GREEN |
| PROFILES-BASELINE-SEMANTICS | CLOSED / VERIFIED | PB-1…PB-6 + Web GREEN |
| USERS-CONTRACT-SEPARATION / UCS-1 | CLOSED / VERIFIED | focused + full Web + Ruff + compile GREEN |
| ADMIN-COMPOSITION-BACKEND | CLOSED / VERIFIED | focused + Web GREEN |
| MANAGER-EXACT-SOURCE-BOUNDARY | CLOSED / VERIFIED | focused + Web GREEN |
| USERS-PROFILES-ADMIN-DRAFT-BASELINE-SEMANTICS | CLOSED / VERIFIED | focused + Web GREEN |
| USERS-MANAGER-EXACT-SOURCE-COMPOSITION | CLOSED / VERIFIED | 7 focused + full Web + Ruff GREEN |

## UCS-1 checkpoint

```text
moragaga/atlanticus@05d6cbb5b81b762f7fc06fc96b7959bfb835a7e3
```

Qualification preservada:

```text
31 focused passed
9 Cosmos store hotfix tests passed
552 passed, 7 skipped full Web
Ruff GREEN
productive/commented compile GREEN
git diff --check GREEN
```

## Admin Composition Backend + Manager Exact-Source Boundary

Checkpoint:

```text
moragaga/atlanticus@9342769a626c39d1f7f860f81e051e2ef1300620
```

Propiedades demostradas:
- canonical authoring backend usa `UsersProfilesConfiguration`;
- draft serializa exact `SourceSnapshot`;
- Profile delete/reassign contractual;
- Managed creation desde Pending;
- Manager exact-source protocol opt-in;
- coordinator preserva value objects exactos y adjudica conflictos.

Suite Web observada:

```text
571 passed, 7 skipped
```

## Users Profiles Admin Draft Baseline Semantics

Checkpoint:

```text
moragaga/atlanticus@567e1a12c862b46dfd7f4ec75c3be750c95bbd54
```

Parent:

```text
moragaga/atlanticus@7da55a8fab4aa6d23d4626d937d574e1ea550d7f
```

Estado:

```text
USERS-PROFILES-ADMIN-DRAFT-BASELINE-SEMANTICS
CLOSED / VERIFIED / CURRENT
```

Propiedades demostradas:
- draft schema = `2`;
- `base_payload_revision` obligatorio;
- draft recién creado nace clean;
- `has_local_changes` compara revision vs base revision;
- `with_configuration(...)` preserva base local y exact Source snapshot;
- `rebase(...)` adopta nuevo exact Source snapshot y convierte revision actual en BASE;
- schema 1 no es aceptado por el parser nuevo;
- local revisions no se reinterpretan como Source identity.

Qualification reportada por el usuario:

```text
focused tests    19 passed
Ruff             All checks passed
full Web suite   580 passed, 7 skipped
Python runtime   3.14.7
```

`atlanticus:main` fue posteriormente verificado conteniendo este checkpoint como parent del siguiente.

## Users Manager Exact-Source Composition

Checkpoint:

```text
moragaga/atlanticus@7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98
```

Parent:

```text
moragaga/atlanticus@567e1a12c862b46dfd7f4ec75c3be750c95bbd54
```

Estado:

```text
USERS-MANAGER-EXACT-SOURCE-COMPOSITION
CLOSED / VERIFIED / CURRENT
```

Propiedades demostradas:
- `UsersManagerExactSourceWorkflow` satisface `ExactSourcePublicationWorkflow`;
- `get_source_snapshot()` preserva el exact value object;
- publication conserva expected `ConcurrencyToken`;
- publication normal conserva `basis_release` desde el snapshot esperado;
- first publish conserva token/basis `None`;
- payload canónico inválido falla antes de Source publication;
- stale exact snapshot falla antes de Source publication;
- `ExactSourcePublicationResult.source` conserva `PublishResult`;
- audit actor se normaliza;
- audit timestamp proviene de la release publicada;
- composition no necesita que Manager dependa de Users;
- composition no necesita que Users dependa de Manager.

Qualification reportada por el usuario:

```text
uv lock --check                         GREEN
focused composition tests              7 passed
Ruff src/tests                          All checks passed
full Web suite                          587 passed, 7 skipped
git diff --check                        GREEN
```

`atlanticus:main` fue verificado apuntando exactamente a `7ffebdbb...`.

## Límite no demostrado por `7ffebdbb...`

El checkpoint **no demuestra** productivo exact-source cutover.

La inspección read-only del host ADA muestra que todavía se registra:

```text
UsersManagerWorkflowAdapter(dependencies.users)
```

Ese adapter usa:
- `UsersConfigurationCatalog`;
- `expected_source_revision: str | None`.

Por tanto:

```text
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER
PLANNED / UNVERIFIED
```

No usar la existencia del package `users-manager` como evidencia de que callbacks/publication productivos ya usan `publish_draft_exact(...)`.

## No demostrado / no cerrado

- callbacks/layout/browser store usando `UsersProfilesAdminDraft` schema 2;
- comportamiento UI ante draft legacy incompatible;
- service registration productivo del exact-source workflow Users;
- runtime canonical cutover;
- provenance exact-release en `users.runtime`;
- eliminación legacy;
- resource topology físico de `CosmosUsersConfigurationProjectionStore`;
- fuente física de `BootstrapRootPolicy`;
- mapping exacto Entra;
- Local/John/Jane runtime final;
- migración global Python 3.14.7.

## Git / CI

Git continúa READ ONLY para este cierre documental.

No se afirma CI remoto adicional para `7ffebdbb...`.

La qualification citada proviene del workspace real reportado por el usuario y de inspección read-only del commit integrado.
