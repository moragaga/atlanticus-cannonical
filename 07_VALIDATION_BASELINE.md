# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades.

No reinterpretar un FAIL histórico como fallo vigente sin revisar su adjudicación.

## Checkpoints principales previos preservados

| Hito | Estado | Evidencia resumida |
|---|---|---|
| WEB-STORAGE-TOPOLOGY / USERS-STORAGE-TOPOLOGY | CLOSED / VERIFIED | focal + Web suite GREEN |
| STORAGE-PREFLIGHT-COSMOS-BRIDGE | CLOSED / VERIFIED | bridge + Storage + Users GREEN |
| COSMOS-USERS-RUNTIME-ADAPTER | CLOSED / VERIFIED | package + integrated Web GREEN |
| USERS-RUNTIME-PROJECTION-BOUNDARY | CLOSED / VERIFIED | focal + Web GREEN |
| USERS-CANONICAL-SOURCE-1 | CLOSED / VERIFIED | Source Users + Web GREEN |
| USERS-CANONICAL-PROJECTION-2 | CLOSED / VERIFIED | exact-release + Cosmos Projection GREEN |
| MANAGER-ROOT-CANONICAL-CUTOVER | CLOSED / VERIFIED | Manager 76 passed + Web GREEN |
| PROFILES-DOMAIN-EXTRACTION | CLOSED / VERIFIED | Profiles + Users + Web GREEN |

El detalle histórico de esos checkpoints permanece en canonical especializado y commits previos.

## Profiles Baseline Semantics

Checkpoint final:

```text
moragaga/atlanticus@3ca92d5579e499dd4ab6413fa6d91c9d296b13c2
```

Estado:

```text
PROFILES-BASELINE-SEMANTICS  CLOSED / VERIFIED / CURRENT
```

### PB-1 — Profiles semantic core

Propiedades demostradas:
- `ProfileCatalog()` es realmente vacío;
- sólo contiene `ProfileDefinition` explícitos;
- no existen defaults especiales Local/Admin/Guest;
- no existe `assignable()`;
- duplicate normalized keys fallan;
- `require()` normaliza;
- Profiles core conserva ownership de modelos/normalizadores/error.

Qualification:
- Profiles tests GREEN;
- Ruff GREEN;
- compile GREEN.

El primer full Web posterior a PB-1 encontró consumidores directos rotos; por eso PB-1 no se consideró integrable aisladamente hasta PB-2.

### PB-2 — Direct consumer reconciliation

Propiedades demostradas:
- Pending materializa `EffectiveUser(profile=None)`;
- Pending funciona con `ProfileCatalog()` vacío;
- Pending usa visual fijo `#FF5722/#FFFFFF`;
- Pending rechaza profile, disabled, local y color overrides;
- Resolved requiere Profile y rechaza key Guest;
- Users resolver no consulta Profiles para Pending;
- Users session snapshot soporta `profile=None` para Pending;
- Users Configuration runtime catalog contiene Administrator + Profiles funcionales;
- Guest durable fields continúan round-trip sin crear Profile runtime;
- Users Configuration Web ya no muestra Local/Guest como Profiles;
- File projection profile catalog está vacío antes de Project;
- después de Project expone únicamente Profiles funcionales;
- API legacy `assignable/custom_profiles/special color properties` quedó eliminada del adapter.

Qualification ejecutada en workspace real:
- residual tests: 5 passed;
- Profiles + Users core + Users Configuration: GREEN;
- Ruff: GREEN;
- production/commented compile: GREEN;
- full Web suite: GREEN, 7 skips, 0 failures/errors.

Checkpoint integrado posterior a PB-2:

```text
moragaga/atlanticus@1f60a1b8cb48d941877c30235332be711f7774c1
```

### PB-3 — Root access contract

Propiedades demostradas:
- `BootstrapRootPolicy` usa `issuer + subject_id + enabled`;
- match exacto;
- Root match no ejecuta fallback;
- no-match/disabled delega al fallback;
- `bootstrap_root=True` sólo admite READY;
- Root no admite `user_id`;
- Access snapshot persiste el flag;
- session key v2 invalida limpiamente snapshot anterior.

Qualification ejecutada en workspace real:
- Identity core tests: 37 passed;
- Ruff: GREEN;
- production/commented compile: GREEN;
- full Web suite: GREEN, 7 skips, 0 failures/errors.

Checkpoint:

```text
moragaga/atlanticus@9d69d955191cdf8320f43a34398b38ffbea8fcc4
```

### PB-6 — Profile service composition cleanup

Propiedades demostradas:
- `create_users_module(runtime)` registra sólo Users runtime;
- Users WebModule no recibe ProfileCatalog;
- `PROFILE_CATALOG_SERVICE_KEY` fue eliminado;
- test focal congela únicamente ownership Users;
- mirror pedagógico acompaña productivo.

Qualification ejecutada en workspace real:
- Users core: 37 passed;
- Ruff: GREEN;
- production/commented compile: GREEN;
- full Web suite: GREEN, 7 skips, 0 failures/errors.

Checkpoint final:

```text
moragaga/atlanticus@3ca92d5579e499dd4ab6413fa6d91c9d296b13c2
```

## Propiedades finales demostradas

```text
Profiles core
→ no special identities

Pending
→ Users
→ profile=None

Administrator/custom
→ explicit functional Profiles

Root
→ Identity/bootstrap
→ bootstrap_root=True
→ no user_id

Users WebModule
→ owns Users runtime only
```

## No demostrado por este hito

- fuente física de `BootstrapRootPolicy`;
- mapping exacto de claims Entra;
- Local/John/Jane runtime contract final;
- separación durable Users/Profiles;
- Profiles Source/Projection propia;
- profile deletion/orphan prevention final;
- provenance exact-release de `users.runtime`;
- Users administrative canonical cutover;
- physical resource topology del canonical Users Projection store;
- migración global Python 3.14.7.

## Git / CI

La qualification reportada para PB-1…PB-6 proviene del workspace real del Project.

No se afirma un CI remoto adicional salvo evidencia explícita.

Git continúa READ ONLY para este cierre documental.
