# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades.

No reinterpretar un FAIL histórico como fallo vigente sin revisar su adjudicación.

No declarar GREEN global cuando sólo existe qualification scoped.

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
| ADMIN-UI-DRAFT-CUTOVER | CLOSED / VERIFIED | Users focal + ADA scoped + full Web + Ruff + lock GREEN |

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

Qualification preservada:

```text
focused tests    19 passed
Ruff             All checks passed
full Web suite   580 passed, 7 skipped
Python runtime   3.14.7
```

## Users Manager Exact-Source Composition

Checkpoint:

```text
moragaga/atlanticus@7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98
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

Qualification preservada:

```text
uv lock --check                         GREEN
focused composition tests              7 passed
Ruff src/tests                          All checks passed
full Web suite                          587 passed, 7 skipped
git diff --check                        GREEN
```

## Admin UI Draft Cutover

Checkpoint:

```text
moragaga/atlanticus@d23bff025ab899367a8da1178dde5ab50806fe47
```

Parent:

```text
moragaga/atlanticus@7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98
```

Estado:

```text
ADMIN-UI-DRAFT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Propiedades demostradas:
- active Users admin Web exports/registers canonical layout/callbacks;
- editor payload es `UsersProfilesConfiguration`;
- draft basis/save usa `UsersProfilesAdminDraft` schema 2;
- exact `SourceSnapshot` se preserva durante edición/save local;
- schema 1 browser draft se rechaza y se reconstruye clean desde current Source;
- no se fabrica provenance al recuperar draft incompatible;
- Administrator edit usa operación canónica;
- Profile create/edit usa operaciones canónicas;
- delete de Profile referenciado no se ejecuta desde UI actual;
- alta Managed parte de Pending y revalida Pending;
- edit Managed preserva identidad;
- import file legacy se convierte explícitamente al payload canónico sin convertirse en browser-draft migration;
- `UsersAdminWebContext` depende de `UsersProfilesAdministrationService`;
- ADA composition exige `users_profiles_administration` para Users UI;
- mirrors productivos/comentados del alcance modificado quedaron equivalentes donde aplica.

Qualification observada:

```text
Users focal pytest                         15 passed
Users Ruff                                 All checks passed
ADA scoped composition test                4 passed
ADA scoped composition + mirror            5 passed
ADA scoped Ruff                            All checks passed
full Web suite                             587 passed, 7 skipped
full Web Ruff                              All checks passed
web uv lock --check                        GREEN / 75 packages resolved
git diff --check                           GREEN
Python runtime usado                       3.14.2
```

`atlanticus:main` fue verificado read-only apuntando exactamente a `d23bff...`.

No se afirma CI remoto adicional.

## ADA host qualification caveat

### Frozen lock/source drift — VERIFIED

En `ada-configuration-manager`:

```text
lock:   atlanticus-web-manager             0.3.14
source: atlanticus-web-manager             0.3.15

lock:   atlanticus-web-users-configuration 0.1.6
source: atlanticus-web-users-configuration 0.1.9
```

Con `uv run --frozen`, el host llegó a importar source Manager actual con metadata/dependencias resueltas desde el lock viejo y falló por `ModuleNotFoundError: atlanticus.web.projection`.

### Residuo local de entorno — RESOLVED LOCALLY

Se verificó un editable local residual:

```text
ada-web-tool-configuration-editor 0.2.0
```

interceptando el namespace `ada.web.configuration`.

Se eliminó únicamente del `.venv`; luego `ada.web.configuration` resolvió al package correcto `scopes/ada/web/configuration/core` y expuso `ConfigurationPageRequest`.

Esto no es cambio de repositorio ni decisión arquitectónica.

### Overlay actual de Manager/Users — EVIDENCIA PARCIAL

Con overlay efímero de los packages locales actuales:

```text
tests/test_composition.py                              4 passed
tests/test_commented_mirror.py + test_composition.py  5 passed
```

La suite ADA completa bajo ese overlay produjo antes del fix de mirror:

```text
54 passed
6 failed
```

Adjudicación:
- 1 failure pertenecía al mirror del propio UI cutover; se corrigió y quedó GREEN en la suite scoped posterior;
- 5 failures correspondían a adapters ADA legacy incompatibles con el contrato Manager actual `0.3.15` (`ProjectionTarget`, `ProjectionExecutionResult.target`, runtime protocol).

La suite ADA completa no se volvió a ejecutar después del fix del mirror. Por tanto:

```text
ADA full suite against current Manager 0.3.15
UNVERIFIED / NOT GREEN AS LAST OBSERVED
```

No usar este resultado para ampliar retroactivamente `ADMIN-UI-DRAFT-CUTOVER` hacia KPI/Navigation/Tools.

## Límite no demostrado por `d23bff...`

El checkpoint **no demuestra**:
- productivo exact-source Manager Users cutover;
- publication action productiva mediante `publish_draft_exact(...)`;
- wiring físico externo real de `users_profiles_administration`;
- UX de replacement al borrar Profile referenciado;
- full ADA host compatibility con Manager 0.3.15;
- runtime canonical cutover;
- provenance exact-release en `users.runtime`;
- eliminación legacy;
- resource topology físico de `CosmosUsersConfigurationProjectionStore`;
- fuente física de `BootstrapRootPolicy`;
- mapping exacto Entra;
- Local/John/Jane runtime final;
- qualification de `d23bff...` bajo Python 3.14.7.

## Python baseline

Decisión Project:

```text
Python 3.14.7
python:3.14.7-slim-trixie
```

Cierre `d23bff...`:

```text
uv python find 3.14.7   no interpreter found
python3 --version        Python 3.14.2
```

Por tanto la qualification específica de este checkpoint en 3.14.7 está BLOCKED / UNVERIFIED.

Esto no invalida la qualification funcional realizada en 3.14.2, pero tampoco debe presentarse como prueba del baseline final.

## Git / CI

Git continúa READ ONLY para este cierre documental.

No se afirma CI remoto adicional para `d23bff...`.

La qualification citada proviene del workspace real reportado por el usuario y de inspección read-only de `atlanticus:main`.
