# Web Platform — Users / Profiles / Navigation Capability Boundary

Estado: **CURRENT DECISION / IMPLEMENTATION IN PROGRESS**

## Propósito

Este documento fija la frontera vigente entre:

```text
Users
Profiles
Navigation
```

y sirve como checkpoint canónico para el cutover incremental.

## Autoridad de implementación

CURRENT inspeccionado:

```text
moragaga/atlanticus:main
4e008055ddc551e6c08a7d87715340c8c7cd149e
```

Parent inmediato:

```text
709cf2fb9ee422094f011cfda051f08f37276992
```

Git permanece SOLO LECTURA para el asistente.

## Estado del frente

```text
USERS-PROFILES-NAVIGATION-CAPABILITY-BOUNDARY
IN PROGRESS

USERS-STANDALONE-AUTHORITY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
IN PROGRESS

USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
PLANNED / NEXT

USERS-PROFILES-COMPOSITION-CUTOVER
PLANNED

PROFILES-UI-EXTRACTION
PLANNED

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
PLANNED

QUALIFICATION
PLANNED
```

## Regla principal

Atlanticus es una base genérica.

El target de composition queda:

```text
Users
  │ standalone válido
  ▼
Profiles
  │ requiere Users
  ▼
Navigation
    requiere Profiles
    y por transitividad Users
```

Combinaciones válidas:

```text
Users
Users + Profiles
Users + Profiles + Navigation
```

Combinaciones inválidas:

```text
Profiles without Users
Navigation without Profiles
Users + Navigation without Profiles
```

La dependencia funcional no obliga a acoplar innecesariamente los cores.

## Semántica estructural

Misma responsabilidad implica mismo concepto.

Target:

```text
users/
├── core
└── configuration

profiles/
├── core
└── configuration

navigation/
├── core
└── configuration
```

No crear arquitectura especial para Profiles.

La propuesta:

```text
profiles/management
```

queda:

```text
SUPERSEDED / NOT ADOPTED
```

## Users

Users puede existir standalone.

Users core posee autoridades base:

```text
guest
basic
root
local
```

Contratos:

```text
guest
TRANSITIONAL / NON-ASSIGNABLE

basic
ASSIGNABLE / STANDARD

root
ASSIGNABLE / SYSTEM FULL AUTHORITY

local
LOCAL-RUNTIME ONLY / NON-ASSIGNABLE / FULL AUTHORITY
```

`administrator` no pertenece al contrato final.

```text
administrator
REMOVE
```

No existe compatibilidad permitida:

```text
administrator -> root
```

### Local identities

```text
Jane Doe
authority = local
avatar background = #C85D91
avatar text = #FFFFFF

John Doe
authority = local
avatar background = #3778C2
avatar text = #FFFFFF
```

Guest:

```text
background = #FF5722
text = #FFFFFF
```

Jane y John son identities, no Profiles.

CURRENT contiene selector local para ambas identities.

El wiring exacto de ese selector en el composition root ejecutado permanece
`UNVERIFIED`.

## Profiles

Profiles es first-class capability.

CURRENT implementado:

```text
web/capabilities/profiles/core
web/capabilities/profiles/configuration
```

Ownership:

```text
profiles/core
ProfileDefinition
ProfileCatalog
profile domain invariants

profiles/configuration
ProfilesConfiguration
```

`ProfilesConfiguration` ya no vive dentro de core.

Target todavía pendiente:

```text
Profiles Source
Profiles Projection/configuration lifecycle
Profiles administration
Profiles UI
```

Profiles requiere Users a nivel de composition.

Esto no autoriza a Profiles a apropiarse de identidad, pending users,
UsersRuntimeStore o lifecycle de Users.

## Functional profiles

Profiles agrega autoridades funcionales configurables sobre el contrato base de
Users.

Ejemplos son no canónicos:

```text
operator
viewer
engineer
planner
```

La configuración real determina cuáles existen.

Assignable base:

```text
basic
root
```

No asignables:

```text
guest
local
```

Con Profiles:

```text
basic
root
+ configured functional authorities
```

La validación exacta entre Users authority y Profiles configuration pertenece al
`USERS-PROFILES-COMPOSITION-CUTOVER`.

## Navigation

Navigation requiere Profiles en el contract de composition Atlanticus.

```text
Navigation
    ↓
Profiles
    ↓
Users
```

Navigation core debe permanecer desacoplado cuando sea posible.

Preferir:

```text
principal.access_key
```

como boundary efectivo de autorización antes que importar clases concretas de
Profiles.

## CURRENT combinado todavía existente

En `main@4e008055...` todavía existe:

```text
UsersProfilesConfiguration
UsersProfilesAdministrationService
UsersProfilesAdminDraft
```

El combined configuration todavía usa:

```text
user.profile_key
administrator
```

y `UsersProfilesConfiguration` sigue validando Users contra
`ProfilesConfiguration`.

Por tanto:

```text
Profiles ownership extraction
PARTIAL / IN PROGRESS
```

## Source / Projection

Target:

```text
Users
source_key = users
payload = UsersConfiguration

Profiles
source_key = profiles
payload = ProfilesConfiguration

Navigation
source_key = navigation
```

Cada capability publica su propio Source.

No recrear publicación atómica Users + Profiles mediante transacción distribuida.

CURRENT todavía conserva Users + Profiles en un mismo lifecycle.

Siguiente corte:

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
```

Debe reemplazar ese ownership combinado de raíz, no crear un Source paralelo
mientras el viejo siga vigente.

## Users configuration field

Target decidido:

```text
UserConfiguration.authority_key
```

CURRENT todavía:

```text
UserConfiguration.profile_key
```

Estado:

```text
DECIDED / NOT YET IMPLEMENTED
```

No crear alias de ambos nombres.

## UI

Target:

```text
Users UI
ownership = Users

Profiles UI
ownership = Profiles

Navigation UI
ownership = Navigation
```

Profiles UI todavía permanece combinada con Users y se corta en un incremento
posterior.

No mezclar UI extraction con Source ownership.

## Publication consistency

Separar Users y Profiles elimina la publicación atómica del agregado actual.

No recrear distributed transaction.

La integridad cross-capability debe resolverse mediante secuencias explícitas,
recovery y auditoría cuando el caso operacional lo exija.

Delete/reassign de un functional profile referenciado sigue OPEN para el
composition cutover.

## Testing rules

Automatizar:

```text
behavior
contracts
invariants
regressions
critical flows
```

No automatizar como contract tests:

```text
CSS visual
responsive
spacing
branding
source token scans
import scans
existence/non-existence of functions/classes
AST/module structure
implementation internals
```

Assets JS/CSS sólo se verifican por carga/existencia si el contrato lo requiere.

Existe un test CURRENT que escanea source para validar ausencia de Profiles en
Users core. Es un conflicto conocido de test hygiene y no debe replicarse.

## Conflicto canonical previo

`15_WEB_PLATFORM/01_CAPABILITY_INDEPENDENCE.md` en el checkpoint
`497207bb...` todavía decía:

```text
Navigation can exist without Users/Profile
profile binding optional
```

Eso queda `SUPERSEDED`.

El reemplazo companion de ese documento debe reflejar:

```text
technical core independence
!=
valid composition independence
```

Aplicados juntos los reemplazos de este cierre, el conflicto queda resuelto
documentalmente.

## Reglas congeladas

```text
Atlanticus generic
REQUIRED

same responsibility -> same concept/naming
REQUIRED

Users standalone
REQUIRED

Users -> Profiles core dependency
FORBIDDEN

Profiles first-class capability
REQUIRED

Profiles structure
core + configuration

Profiles -> Users
REQUIRED AT COMPOSITION

Navigation -> Profiles
REQUIRED AT COMPOSITION

Navigation -> Users direct ownership
FORBIDDEN

guest
TRANSITIONAL / NON-ASSIGNABLE

basic
ASSIGNABLE / STANDARD

root
ASSIGNABLE / FULL AUTHORITY

local
LOCAL-RUNTIME ONLY / NON-ASSIGNABLE / FULL AUTHORITY

administrator
REMOVE

Jane/John local identities/colors
PRESERVE

UsersProfiles aggregate contracts
REMOVE

Profiles UI inside Users
REMOVE

LEGACY
REMOVE

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN

CSS VISUAL CONTRACT TESTS
FORBIDDEN
```

## Orden de implementación refinado

```text
1. USERS-STANDALONE-AUTHORITY-CUTOVER
   CLOSED / VERIFIED / CURRENT

2A. PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
    CLOSED / VERIFIED / CURRENT

2B. USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
    PLANNED / NEXT

3. USERS-PROFILES-COMPOSITION-CUTOVER
   PLANNED

4. PROFILES-UI-EXTRACTION
   PLANNED

5. NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
   PLANNED

6. QUALIFICATION
   PLANNED
```

## Criterio de cierre del frente completo

No declarar CLOSED hasta que:

```text
Users funciona standalone
Profiles tiene ownership y lifecycle propios
UsersProfiles aggregate no existe
combined Users+Profiles Source no existe
UserConfiguration usa authority_key final
administrator no existe
Profiles UI está fuera de Users
Navigation se compone con Profiles + Users
no existen shims/aliases/adapters legacy
tests validan comportamiento/invariantes
```

## Pendientes explícitos

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
OPEN / NEXT

USERS-PROFILES-COMPOSITION-CUTOVER
OPEN

PROFILES-UI-EXTRACTION
OPEN

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
OPEN

local selector composition wiring
UNVERIFIED

referenced profile delete/reassign recovery/audit
OPEN

Python 3.14.7 metadata alignment
OPEN / SEPARATE

WEB-TEST-CONTRACT-CLEANUP
OPEN / SEPARATE
```
