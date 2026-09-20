# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@31723a108ddd2f49346fdcbb844db9891eb08f4b
```

Parent inmediato:

```text
29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
```

Tree:

```text
4213a0dd11abbc9cb22fb02ed4a60d08bf0f87c5
```

Canonical inspeccionado para este cierre:

```text
moragaga/atlanticus-cannonical@a48ae6d1433b5ae39288d3b41002782efa10c9cd
```

Git permanece SOLO LECTURA para el asistente.

## Estado resumido

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT                CLOSED / VERIFIED / CURRENT
PROFILES-MANAGER-COMPOSITION                          CLOSED / VERIFIED / CURRENT
USERS-PROFILES-CONTRACT-REALIGNMENT                   CLOSED / VERIFIED / CURRENT
USERS-ADMINISTRATION-MANAGER-INTEGRATION              CLOSED / VERIFIED / CURRENT
ADA-ACCESS-PROJECTION-PERSISTENCE                     CLOSED / VERIFIED / CURRENT
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION          CLOSED / VERIFIED / CURRENT
ACCESS-UNRESTRICTED-PROFILES-CONTRACT                 CLOSED / VERIFIED / CURRENT
ACCESS-MANAGER-UI-REVIEW                              CLOSED / VERIFIED MANUAL / CURRENT
MANAGER-FINAL-ADMIN-COMPOSITION                       CLOSED / VERIFIED / CURRENT

NAVIGATION-STANDALONE-CONFIGURATION-CUTOVER           CLOSED / VERIFIED / CURRENT
NAVIGATION-PUBLIC-ACCESS-CONTRACT                     CLOSED / VERIFIED / CURRENT
NAVIGATION-PROFILE-OPTIONS-DECOUPLING                 CLOSED / VERIFIED / CURRENT
NAVIGATION-CONFIGURATION-UI-PASS                      CLOSED / VERIFIED MANUAL / CURRENT

MANAGER-UI-CONSISTENCY-REVIEW                         IN PROGRESS
MANAGER-RESPONSIVE-MEDIA-QUERY-AUDIT                  PLANNED / PHASE 2
WEB-TEST-CONTRACT-CLEANUP                             PLANNED / PHASE 3

NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT   BLOCKED / VERIFIED CONFLICT
MANAGER-REAL-PERSISTENCE-QUALIFICATION                PLANNED / AFTER UI REVIEW
PYTHON-METADATA-ALIGNMENT                             PLANNED / SEPARATE
```

## VERIFIED

### ADA Access unrestricted profile semantics

CURRENT:

```text
root
→ todos los access_keys definidos
→ grants explícitos rechazados

local
→ todos los access_keys definidos
→ grants explícitos rechazados

basic / guest / custom
→ grants explícitos configurables
```

`AdaAccessConfiguration` sigue persistiendo solamente:

```text
access_keys
profile_access
```

No se añadió flag `unrestricted`, alias, reader legacy ni segundo schema.

### ADA Access admin UI

CURRENT:

```text
tabs
Accesos / Perfiles

access creation UI
Ámbito + Permiso -> access_key

durable access identity
single string access_key

pagination
atlanticus.web.pagination

page sizes
10 / 20

profiles fallback without active Profiles Projection
ProfileCatalog() system catalog

assignable system profiles in fallback
basic / guest

root / local
not assignable in UI

profile assignment row
stable summary + Configurar

profile editor
viewport-centered modal + dbc.Checkbox

no access keys
Configurar disabled; empty modal does not open

assignment state
directly in editable AdaAccessConfiguration

horizontal overflow
cause-contained at Dash page-size control

footer
aligned with qualified Manager surfaces
```

La UI de Access conserva la reserva vertical paginada usada en las surfaces ya corregidas.

Desktop, mobile, modal y estado sin accesos fueron aceptados manualmente por el usuario antes
del checkpoint CURRENT.

### Profiles title in ADA Configuration Manager

La composition local ADA invoca:

```text
compose_profiles_manager(..., title='Perfiles', ...)
```

Esto cambia el título visible en la aplicación ADA sin cambiar el default generic
`title='Profiles'` de `compose_profiles_manager`.

### Navigation runtime authorization semantics

CURRENT:

```text
disabled
→ deny

enabled + allowed_profiles = ()
→ allow for restricted principals

enabled + allowed_profiles = non-empty
→ require principal.access_key membership

principal.unrestricted
→ allow profile restriction bypass, except disabled route remains denied
```

Home path remains allowed by Navigation authorization.

Unknown internal paths remain denied for restricted principals.

### Navigation Configuration capability boundary

CURRENT implementation does not import or depend on:

```text
atlanticus.web.profiles
atlanticus.web.users
ada.*
```

Neutral contract:

```text
NavigationProfileOption(key, label)
NavigationProfileOptionsProvider
```

Provider is optional.

Validation of referenced profile keys is installed only when a provider is supplied.

### Shared Manager CSS finding

CURRENT implementation contains:

```text
.atlanticus-manager__module-page {
    padding-block: 1rem 0;
}

@media (max-width: 48rem) {
    .atlanticus-manager__module-page {
        padding-block: .75rem 0;
    }
}
```

This is implemented reality.

Its correctness across all Manager pages and breakpoints is not yet qualified as a shared
responsive decision.

## Qualification evidence

Durante Access se observó:

```text
ADA Access Configuration
46 passed + 1 failing test
```

El fallo correspondía únicamente a `dash.html.Input`. La implementación se corrigió a
`dbc.Checkbox`, y el usuario confirmó posteriormente que todo quedó OK antes de publicar
`31723a1...`.

También se observó:

```text
ADA Configuration Manager
31 passed

git diff --check
PASS
```

El Ruff package-wide de `ada-configuration-manager` reportó tres `I001` en:

```text
kpi_definitions.py
kpis.py
workflows.py
```

Esos archivos no forman parte del commit CURRENT y no fueron modificados en este hito.

Permanece UNVERIFIED:

```text
remote CI
full monorepo pytest
full workspace Ruff
```

## INFERRED

No se necesita arquitectura nueva para revisar la página Manager de Perfiles.

Las correcciones del siguiente foco deberían permanecer en la capability/composition que las
posee, salvo que se demuestre un defecto transversal real de Manager.

## ASSUMED

No se asume que la UI de Perfiles esté correcta por compartir shell con Navigation o Access.

No se asume que los tres `I001` externos deban resolverse durante la revisión de Perfiles.

## PROPOSED

Single next focus:

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS / NEXT PAGE: PERFILES
```

Orden:

```text
PHASE 1
revisar Perfiles visualmente y corregir sólo su slice

PHASE 2
auditar responsive/media queries cuando las páginas desktop estén coherentes

PHASE 3
qualification final behavior-focused y cleanup de tests inválidos
```

## UNVERIFIED / PENDING

```text
Perfiles final visual consistency
UNVERIFIED / NEXT

Herramienta final visual consistency
OPEN / DEFERRED

remaining Manager pages visual consistency
UNVERIFIED

shared and local media-query correctness
UNVERIFIED

Manager real persistence flows
PLANNED / AFTER UI REVIEW

Navigation Manager authorization consumer alignment
BLOCKED / SEPARATE

CI remote
UNVERIFIED

Python metadata global 3.14.7
PLANNED / SEPARATE
```
