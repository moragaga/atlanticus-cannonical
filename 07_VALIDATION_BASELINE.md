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

### Propiedades demostradas — Profiles durable

- existe `ProfilesConfiguration`;
- contiene sólo Profiles explícitos;
- round-trip durable conserva `ProfileDefinition`;
- duplicate normalized profile keys fallan mediante la semántica existente de Profiles;
- `ProfilesConfiguration.catalog()` no introduce defaults implícitos.

### Propiedades demostradas — Users durable

- existe `UsersConfiguration`;
- contiene Managed Users;
- ids duplicados fallan;
- emails no nulos duplicados fallan;
- identidades `(issuer, subject_id)` duplicadas fallan.

### Propiedades demostradas — composición Users/Profiles

- existe `UsersProfilesConfiguration`;
- exige Profile `administrator`;
- rechaza `guest` y `local` como Profiles funcionales;
- todo Managed User debe resolver `profile_key`;
- la validación aplica también a Users disabled;
- Administrator se representa como Profile explícito.

### Propiedades demostradas — Source

Escritura nueva:

```text
users/configuration.json.gz
profiles/configuration.json.gz
```

- ambos resources pertenecen a la misma exact Source release;
- Users resource escribe schema `2`;
- Profiles resource escribe schema `1`;
- `published_by` se preserva;
- gzip/JSON de tests es determinista;
- nueva escritura no incluye `administrator_*` ni `guest_*` en Users configuration;
- misma configuración puede publicarse como otra release distinta sin colapsar release identity.

Lectura histórica:
- Users source schema `1` continúa legible;
- aggregate legacy se normaliza;
- Administrator colors históricos crean Administrator explícito;
- Guest histórico no crea Profile funcional.

### Propiedades demostradas — Projection

- `UsersProjectionBuilder` produce `UsersProfilesConfiguration`;
- actor Source no entra al payload de Projection;
- `project(target)` usa exactamente la release seleccionada;
- `project(target)` no relee current;
- same content en releases distintas sigue siendo target distinto.

### Propiedades demostradas — Cosmos Projection

- write schema actual = `2`;
- read schema `1` normaliza hacia `UsersProfilesConfiguration`;
- misma exact release + mismo payload es idempotente;
- misma exact release + payload diferente falla por invariantes;
- nueva release reemplaza con CAS/ETag;
- concurrent same-target winner es éxito idempotente;
- concurrent different target produce conflict;
- se puede activar explícitamente una release exacta anterior.

### Qualification ejecutada en workspace real

Focal UCS-1:

```text
31 passed
```

Hotfix de estilo del store test:

```text
9 passed
```

Suite Web completa:

```text
552 passed, 7 skipped
0 failures
0 errors
```

Calidad:

```text
uv run --frozen ruff check .
→ All checks passed!

productive/commented compile
→ GREEN

git diff --check
→ GREEN
```

El hotfix final cambió sólo formato del test `users/projection-cosmos/tests/test_store.py`; no cambió comportamiento.

## No demostrado / no cerrado por UCS-1

- composición administrativa Users/Profiles usando contratos separados;
- runtime canonical cutover desde la Projection nueva;
- provenance exact-release dentro de `users.runtime`;
- migración administrativa Users completa;
- eliminación de legacy Users;
- resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`;
- fuente física de `BootstrapRootPolicy`;
- mapping exacto de claims Entra;
- Local/John/Jane runtime contract final;
- Profiles Source/Projection independiente;
- migración global Python 3.14.7.

Sobre Profiles Source/Projection independiente: UCS-1 demuestra que no fue necesario para separar ownership actual; no se considera requisito pendiente salvo que aparezca una necesidad nueva.

## Git / CI

El checkpoint `05d6cbb5b81b762f7fc06fc96b7959bfb835a7e3` está verificado como tip de `moragaga/atlanticus:main` durante este cierre.

La qualification reportada proviene del workspace real del Project.

No se afirma CI remoto adicional salvo evidencia explícita.

Git continúa READ ONLY para este cierre documental.
