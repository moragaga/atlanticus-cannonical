# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@4e008055ddc551e6c08a7d87715340c8c7cd149e
```

Parent inmediato:

```text
709cf2fb9ee422094f011cfda051f08f37276992
```

Canonical inspeccionado para este cierre:

```text
moragaga/atlanticus-cannonical@497207bbdda23a829897751f37b9653298adf514
```

Git permanece SOLO LECTURA para el asistente.

## Estado resumido

```text
USERS-STANDALONE-AUTHORITY-CUTOVER          CLOSED / VERIFIED / CURRENT
PROFILES-CONFIGURATION-BOUNDARY-CUTOVER     CLOSED / VERIFIED / CURRENT
PROFILES-CAPABILITY-EXTRACTION              IN PROGRESS
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER     PLANNED / NEXT
USERS-PROFILES-COMPOSITION-CUTOVER          PLANNED
PROFILES-UI-EXTRACTION                      PLANNED
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT    PLANNED
QUALIFICATION                               PLANNED
WEB-TEST-CONTRACT-CLEANUP                   PLANNED / OPEN
```

Los hitos genéricos de Manager, Navigation, Tools, KPI Configuration,
KPI Definition y ADA Configuration Manager cerrados anteriormente permanecen
`CLOSED / VERIFIED / CURRENT`.

## VERIFIED

### Published checkpoint

`main` está publicado en:

```text
4e008055ddc551e6c08a7d87715340c8c7cd149e
```

con parent inmediato:

```text
709cf2fb9ee422094f011cfda051f08f37276992
```

### Users standalone authority

El checkpoint parent introdujo en `users/core` el contrato de autoridad base:

```text
guest
basic
root
local
```

Semántica implementada en core:

```text
guest  non-assignable
basic  assignable
root   assignable / full access
local  non-assignable / full access
```

`EffectiveUser` y `ResolvedUserRecord` usan `authority_key` en el runtime de Users.

Users core dejó de requerir objetos `ProfileDefinition` / `ProfileCatalog`
para resolver usuarios.

Existe un selector local con Jane Doe y John Doe y sus colores definidos.

Qualification observada antes de publicar `709cf2f...`:

```text
users/core                 41 PASS
users/cosmos               22 PASS
users/projection-cosmos    29 PASS
TOTAL                      92 PASS

git diff --check           PASS
```

### Profiles configuration boundary

`ProfilesConfiguration` ya no pertenece a `profiles/core`.

CURRENT:

```text
web/capabilities/profiles/core
    domain/core

web/capabilities/profiles/configuration
    ProfilesConfiguration
```

Existe el package:

```text
atlanticus-web-profiles-configuration==0.1.0
```

`users/configuration` declara explícitamente esa dependencia mientras todavía
consume `ProfilesConfiguration`.

Qualification observada antes de publicar `4e008055...`:

```text
profiles/core              7 PASS
profiles/configuration     2 PASS
users/configuration       65 PASS
TOTAL                     74 PASS

uv lock
PASS

git diff --check
PASS
```

## INFERRED

La nueva frontera `profiles/core + profiles/configuration` alinea Profiles con
la misma semántica estructural usada por otras capabilities sin crear un package
especial.

Esto no demuestra todavía que Profiles tenga source, projection, administration
o UI independientes.

## ASSUMED

No se asume:

- que `UsersProfilesConfiguration` haya sido eliminado;
- que Users y Profiles ya publiquen Sources independientes;
- que `profile_key` haya desaparecido de Users configuration;
- que `administrator` haya desaparecido del agregado combinado CURRENT;
- que Profiles UI ya sea independiente;
- que Navigation ya exija Profiles en composition;
- que el selector local Jane/John esté conectado al composition root ejecutado;
- que full workspace pytest/Ruff haya pasado en este checkpoint;
- que CI remoto haya pasado;
- que metadata Python 3.14.7 esté alineada globalmente.

## PROPOSED

Único foco siguiente:

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
PLANNED / NEXT
```

Debe partir de código CURRENT e implementar únicamente la separación real de
ownership Source/Projection/configuration entre Users y Profiles.

## SUPERSEDED / REFINED

### Profiles management package

La propuesta transitoria:

```text
web/capabilities/profiles/management
```

queda:

```text
SUPERSEDED / NOT ADOPTED
```

Profiles mantiene la semántica:

```text
profiles/
├── core
└── configuration
```

`management` no se usa como sinónimo genérico de configuration.

### ProfilesConfiguration dentro de core

```text
profiles/core/.../configuration.py
SUPERSEDED / REMOVED
```

El owner CURRENT es:

```text
profiles/configuration
```

### Optional Navigation/Profile composition

La regla canónica anterior que permitía Navigation sin Profiles queda
`SUPERSEDED BY CURRENT DECISION`.

Target vigente:

```text
Users
  ↓
Profiles
  ↓
Navigation
```

La dependencia funcional no obliga a introducir imports innecesarios entre
cores.

## UNVERIFIED / OPEN

### Source / aggregate ownership

CURRENT todavía contiene:

```text
UsersProfilesConfiguration
UsersProfilesAdministrationService
UsersProfilesAdminDraft
combined Users + Profiles Source
combined Users + Profiles Projection payload
```

CURRENT `UsersProfilesConfiguration` todavía requiere:

```text
administrator
```

y valida Users contra:

```text
user.profile_key
```

Por tanto, la extracción de Profiles está sólo parcialmente implementada.

### Users configuration field

`UserConfiguration.profile_key` sigue CURRENT.

Target decidido:

```text
UserConfiguration.authority_key
```

pero todavía no está implementado en esta capa.

### administrator

Users runtime base ya usa `root`, pero el agregado Users/Profiles CURRENT todavía
contiene semántica `administrator`.

Estado:

```text
administrator
DECIDED REMOVE / NOT YET FULLY IMPLEMENTED
```

No crear mapping `administrator -> root`.

### Local runtime wiring

Existe `select_local_user()` con Jane/John.

No está verificado en este cierre qué composition root ejecutado consume ese
selector.

### Test fuera de política

CURRENT contiene:

```text
web/capabilities/users/core/tests/test_authority.py
test_users_core_has_no_profiles_dependency
```

Ese test inspecciona `pyproject.toml` y source text para comprobar ausencia de
dependencias.

Contradice la política CURRENT de tests.

Estado:

```text
OPEN / PLANNED UNDER WEB-TEST-CONTRACT-CLEANUP
```

No mezclar su cleanup con el siguiente source ownership cutover y no crear más
tests de ese tipo.

### Python metadata

Baseline global:

```text
Python 3.14.7
python:3.14.7-slim-trixie
```

Packages CURRENT todavía contienen metadata `requires-python ==3.14.2`.

Permanece OPEN y fuera de este frente.

## Conflictos canonical detectados

Los siguientes documentos del canonical checkpoint `497207bb...` quedaron
desactualizados:

```text
15_WEB_PLATFORM/01_CAPABILITY_INDEPENDENCE.md
15_WEB_PLATFORM/09_CURRENT_GAPS.md
15_WEB_PLATFORM/11_OPEN_ITEMS.md
15_WEB_PLATFORM/12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md
```

`01_CAPABILITY_INDEPENDENCE.md` contradice explícitamente el target
Navigation => Profiles => Users.

`09_CURRENT_GAPS.md` y `11_OPEN_ITEMS.md` describen checkpoints y next steps
anteriores a los cutovers publicados.

`12_USERS_PROFILES_NAVIGATION_CAPABILITY_BOUNDARY.md` conserva el baseline
`d3883e1e...` y marca ambos primeros incrementos como PLANNED.

## Siguiente frontera

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
PLANNED / NEXT
```

No mezclar:

```text
Profiles UI
Navigation dependency alignment
Python metadata
E2E
CSS visual tests
transversal test cleanup
```
