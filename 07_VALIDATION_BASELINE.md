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
| USERS-CONTRACT-SEPARATION / UCS-1 | CLOSED / VERIFIED | 31 focused + full Web + Ruff + compile GREEN |
| ADMIN-COMPOSITION-BACKEND | CLOSED / VERIFIED | 14 focused admin composition tests within 19 new focused tests |
| MANAGER-EXACT-SOURCE-BOUNDARY | CLOSED / VERIFIED | 5 focused Manager exact-source tests within 19 new focused tests |

El detalle histórico de checkpoints anteriores permanece en canonical especializado y commits previos.

## Profiles Baseline Semantics

Checkpoint final previo a UCS-1:

```text
moragaga/atlanticus@3ca92d5579e499dd4ab6413fa6d91c9d296b13c2
```

Estado:

```text
PROFILES-BASELINE-SEMANTICS  CLOSED / VERIFIED / CURRENT
```

Propiedades finales preservadas:
- `ProfileCatalog()` es vacío;
- sólo contiene `ProfileDefinition` explícitos;
- no fabrica Local/Admin/Guest;
- Pending pertenece a Users y usa `profile=None`;
- Guest no es Profile runtime;
- Administrator es Profile funcional;
- Root pertenece Identity/bootstrap;
- Users WebModule registra únicamente Users runtime.

## UCS-1 — Canonical Contract Split

Checkpoint integrado:

```text
moragaga/atlanticus@05d6cbb5b81b762f7fc06fc96b7959bfb835a7e3
```

Parent:

```text
moragaga/atlanticus@3ca92d5579e499dd4ab6413fa6d91c9d296b13c2
```

Estado:

```text
USERS-CONTRACT-SEPARATION        CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT   CLOSED / VERIFIED / CURRENT
```

Qualification UCS-1 preservada:

```text
31 focused passed
9 Cosmos store hotfix tests passed
552 passed, 7 skipped full Web
Ruff GREEN
productive/commented compile GREEN
git diff --check GREEN
```

## Admin Composition Backend + Manager Exact-Source Boundary

Checkpoint integrado:

```text
moragaga/atlanticus@9342769a626c39d1f7f860f81e051e2ef1300620
```

Parent:

```text
moragaga/atlanticus@05d6cbb5b81b762f7fc06fc96b7959bfb835a7e3
```

Estados:

```text
USERS-PROFILES-ADMIN-COMPOSITION  IN PROGRESS
ADMIN-COMPOSITION-BACKEND         CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY     CLOSED / VERIFIED / CURRENT
```

### Propiedades demostradas — admin composition

- `UsersProfilesAdminDraft` usa `UsersProfilesConfiguration`;
- draft serializa `SourceSnapshot` exacto;
- draft revision se deriva del contenido canónico;
- draft inválido por revision mismatch falla;
- nuevo draft tiene document type/schema propio;
- payload legacy no es aceptado por el parser canónico;
- default composition contiene Administrator explícito;
- Administrator no puede eliminarse;
- edición Administrator actual preserva key/label;
- Profile edit preserva key;
- Profile referenciado no puede borrarse sin replacement;
- replacement reasigna Users y elimina Profile en una transformación válida;
- la reasignación incluye Users disabled;
- alta Managed parte de `PendingUserRecord`;
- duplicate/configured identity se rechaza;
- identidad Managed existente no puede cambiar;
- pending ya configurado deja de listarse;
- `UsersProfilesAdministrationService` conserva exact Source snapshot;
- source change durante load/publication se detecta;
- publication usa `ConcurrencyToken` y `basis_release`.

### Propiedades demostradas — Manager exact-source

- existe `ExactSourcePublicationWorkflow` runtime-checkable y opt-in;
- workflow legacy que no lo implementa no adquiere automáticamente el nuevo contrato;
- `get_exact_source_snapshot(...)` transporta `SourceSnapshot`;
- `publish_draft_exact(...)` compara el snapshot esperado con current;
- stale snapshot produce `ManagerSourceConflictError`;
- cambio de Source observado después de fallo del workflow se adjudica como conflict;
- `ExactSourcePublicationResult` conserva `PublishResult` tipado;
- no se introduce conversión `SourceReleaseId <-> str`.

### Qualification ejecutada en workspace real

Focused:

```text
19 passed
```

Suites de capabilities afectadas:

```text
Users Configuration tests  GREEN
Manager tests              GREEN
```

Mirror:

```text
test_canonical_commented_mirrors.py
→ GREEN
```

Compile:

```text
productive/commented compile
→ GREEN
```

Ruff final sobre archivos productivos/tests modificados:

```text
ruff check
→ All checks passed!

ruff format --check
→ 7 files already formatted
```

Suite Web completa:

```text
571 passed, 7 skipped
0 failures
0 errors
```

Git:

```text
git diff --check
→ GREEN

12 implementation files expected in the increment
```

Nota de adjudicación:
- el primer Ruff detectó tres import blocks ordenables y formato en `admin_composition.py`;
- se corrigieron;
- el mirror comentado se realineó;
- después se repitió la full Web suite y quedó GREEN.

No se afirma:
- `ruff check .` global nuevo;
- CI remoto adicional;
- UI/browser admin cutover;
- Users↔Manager exact-source wiring productivo.

## No demostrado / no cerrado por este checkpoint

- callbacks/layout/browser store usando `UsersProfilesAdminDraft`;
- política UI concreta ante draft browser legacy incompatible;
- wiring productivo de Users como `ExactSourcePublicationWorkflow`;
- runtime canonical cutover;
- provenance exact-release en `users.runtime`;
- eliminación legacy;
- resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`;
- fuente física de `BootstrapRootPolicy`;
- mapping exacto Entra;
- Local/John/Jane runtime final;
- migración global Python 3.14.7.

## Git / CI

`9342769a626c39d1f7f860f81e051e2ef1300620` está verificado como tip de `moragaga/atlanticus:main` durante este cierre.

La qualification reportada proviene del workspace real del Project.

No se afirma CI remoto adicional.

Git continúa READ ONLY para este cierre documental.
