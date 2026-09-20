# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
```

Parent inmediato:

```text
856498c52f182cd531deae845c25bd51ae2ff4ea
```

Tree:

```text
3f27ad599c6dec610dff5317494a73b276d2ebc4
```

Canonical inspeccionado para este cierre:

```text
moragaga/atlanticus-cannonical@deb493659b41c0d8fea5c70674002486b3b92cbc
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

### ADA composition

ADA Configuration Manager is the integration point that knows Profiles and Navigation
simultaneously.

It maps the active `ProfileCatalog` to `NavigationProfileOption`.

If there is no active Profiles Projection, it falls back to the system `ProfileCatalog`.

The Navigation assignment selector excludes:

```text
root
local
```

Navigation generic does not special-case those keys.

### Navigation admin UI

CURRENT behavior:

```text
profiles card separate
REMOVED

profile assignment
inside link editor only

guest auto-selection
REMOVED

empty allowed profiles
displayed as public

top-level pagination
CURRENT

page sizes
10 / 20

default page size
10

section children
not counted in top-level total

sections
collapsed initially

expanded section
shows all child links

expanded state
ephemeral UI state, not Source
```

The empty state uses the reserved page area and is centered.

The Dash dropdown focus target is width-contained by Navigation's Dash adapter to avoid
horizontal overflow.

User confirmed the Navigation page looked correct on the CURRENT checkpoint.

### Boundary-test correction

`test_navigation_configuration_is_independent_from_profiles_users_and_ada` now parses Python
imports with `ast`.

The previous substring search for `ada.` was SUPERSEDED because it produced a false positive
on UI text such as `configurada.`.

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

Its correctness across Manager pages and breakpoints is not yet qualified.

## Qualification evidence

Observed before the final visual-only patch:

```text
Navigation core                           22 passed
Navigation Configuration                 42 passed
Navigation Manager                       10 passed
ADA Configuration Manager focused         5 passed
TOTAL                                    79 passed
```

Ruff scoped was also observed passing for Navigation Configuration during the increment.

Not observed after the final CURRENT checkpoint:

```text
post-29bb scoped pytest
UNVERIFIED

post-29bb scoped Ruff
UNVERIFIED

remote CI
UNVERIFIED

full monorepo pytest
UNVERIFIED

full workspace Ruff
UNVERIFIED
```

## INFERRED

No new architecture is required to continue the Manager UI review.

The next corrections should remain capability-local unless a genuinely shared Manager surface
defect is demonstrated.

## ASSUMED

No assumption is made that current media queries are correct merely because the desktop page
looks correct.

No assumption is made that the final CURRENT commit is fully qualified by the earlier 79-test
run.

## PROPOSED

Single next focus:

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS / NEXT PAGE: HERRAMIENTA
```

Ordered phases:

```text
PHASE 1
continue visual review page-by-page

PHASE 2
audit responsive/media queries after desktop surfaces are coherent

PHASE 3
run final behavior-focused tests and remove invalid structural/visual tests
```

## UNVERIFIED / PENDING

```text
Herramienta final visual consistency
UNVERIFIED / NEXT

remaining Manager pages visual consistency
UNVERIFIED

shared and local media-query correctness
UNVERIFIED

final post-29bb targeted qualification
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
