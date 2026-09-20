# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

## Autoridad

Implementación publicada CURRENT:

```text
moragaga/atlanticus@df5b99502265758e873e0565abf2176cc617104b
```

Parent inmediato:

```text
31723a108ddd2f49346fdcbb844db9891eb08f4b
```

Tree:

```text
de1151ba72d44bc8ac6b6f2cfd6f57eb7e80c0a0
```

Canonical inspeccionado para este cierre:

```text
moragaga/atlanticus-cannonical@07a0582c7acdd5c9b93f2a1bb02651e8c5302448
```

Git permanece SOLO LECTURA para el asistente.

## Estado resumido

```text
PROFILES-CONFIGURATION-EDITOR-CONTRACT                CLOSED / VERIFIED / CURRENT
PROFILES-MANAGER-COMPOSITION                          CLOSED / VERIFIED / CURRENT
PROFILES-MANAGER-UI-REVIEW                            CLOSED / VERIFIED MANUAL / CURRENT
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

MANAGER-UI-CONSISTENCY-REVIEW                         IN PROGRESS / NEXT PAGE: USERS
USERS-MANAGER-UI-REVIEW                               PLANNED / NEXT
MANAGER-RESPONSIVE-MEDIA-QUERY-AUDIT                  PLANNED / PHASE 2
WEB-TEST-CONTRACT-CLEANUP                             PLANNED / PHASE 3

NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT   BLOCKED / VERIFIED CONFLICT
MANAGER-REAL-PERSISTENCE-QUALIFICATION                PLANNED / AFTER UI REVIEW
PYTHON-METADATA-ALIGNMENT                             PLANNED / SEPARATE
```

## VERIFIED

### Profiles Manager UI

CURRENT en `df5b995...`:

```text
ownership
Profiles capability + profiles-manager composition

source/projection context
visible and composition-driven

pagination
atlanticus.web.pagination
10 / 20
numbered pages
Mostrando X–Y de Z

configured profile preview
single uppercase initial + configured colors

system profiles
basic / root / guest / local

local visual exception
local identities instead of one fixed profile color

local avatar text
first-name initial + last-name initial, uppercase

profile editor
capability-local viewport modal
backdrop + close/cancel/save
live preview + visible hex values

footer
no empty result spacing
```

`ProfileDefinition`, `ProfileCatalog` y `ProfilesConfiguration` no fueron reemplazados ni
extendidos con estado visual.

### Profiles composition metadata

`compose_profiles_manager(...)` conserva defaults generic:

```text
title='Profiles'
description=''
source_name='Profiles Source'
projection_name='Profiles Projection'
```

Providers generic CURRENT:

```text
local provider
Local Source / Local Projection

azure provider
Blob Storage / Cosmos DB
```

ADA local runtime CURRENT:

```text
title
Perfiles

description
Define los perfiles disponibles y su presentación visual dentro del sistema.

source
Local Source

projection
In-process Projection
```

### Profiles manual qualification

El usuario revisó la UI final y declaró Perfiles cerrado sobre el checkpoint CURRENT.

Estado:

```text
PROFILES-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT
```

### Users boundary para el siguiente foco

CURRENT:

```text
Users
ManagerEntry

UsersAdministrationService
discover / promote / update

managed user profile
UserRecord.profile_key

profile catalog
injected ProfileCatalog

local profile
not a managed assignment
```

No inventar Source/Projection para Users.

### Navigation runtime authorization semantics

CURRENT:

```text
disabled
→ deny

enabled + allowed_profiles = ()
→ allow for restricted principals

enabled + allowed_profiles = non-empty
→ require principal access/profile membership

principal.unrestricted
→ allow profile restriction bypass, except disabled route remains denied
```

### ADA Access unrestricted profile semantics

CURRENT:

```text
root / local
→ todos los access_keys definidos
→ grants explícitos rechazados

basic / guest / custom
→ grants explícitos configurables
```

## INFERRED

No se necesita una arquitectura nueva para revisar Users.

La siguiente revisión debe permanecer dentro de Users + users-manager composition salvo que se
demuestre un defecto transversal real de Manager.

## ASSUMED

No se asume que la UI actual de Users esté visualmente correcta por compartir shell Manager.

No se asume que el cierre manual de Perfiles demuestre un PASS de suites automatizadas en el
checkpoint `df5b995...`.

No se asume que `Herramienta` quede qualified por cerrar el alcance actual del Manager después
de Users.

## PROPOSED

Single next focus:

```text
USERS-MANAGER-UI-REVIEW
PLANNED / NEXT
```

Orden del siguiente chat:

```text
1. inspeccionar contrato y surface CURRENT de Users
2. revisar visualmente la página Users
3. acordar cambios capability-locales
4. implementar incrementalmente sólo tras consenso
5. validar y decidir cierre del alcance actual de Manager
```

## UNVERIFIED / PENDING

```text
post-df5b targeted pytest
UNVERIFIED

post-df5b targeted Ruff
UNVERIFIED

remote CI
UNVERIFIED

full monorepo pytest
UNVERIFIED

full workspace Ruff
UNVERIFIED

Users final visual consistency
NEXT / UNVERIFIED

Herramienta final visual consistency
OPEN / DEFERRED

shared and local media-query correctness
UNVERIFIED

Manager real persistence flows
PLANNED / AFTER UI REVIEW

Navigation Manager authorization consumer alignment
BLOCKED / SEPARATE

Python metadata global 3.14.7
PLANNED / SEPARATE
```
