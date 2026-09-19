# Web Platform — Users / Profiles / Access / Navigation Capability Boundary

Estado: **CURRENT DECISION / REFINED AFTER PROFILES, ADA ACCESS AND ACCESS-SEMANTICS CUTOVERS**

## Propósito

Este documento fija la frontera CURRENT entre:

```text
Global Users
Generic Profiles
Application-specific Access
Generic Navigation
```

sin reintroducir Users Configuration Source, estado app-specific dentro de Global User
ni un Access generic no demostrado.

## Autoridad de implementación

CURRENT inspeccionado:

```text
moragaga/atlanticus:main
0fba548329afd9bc9dee92ea6caa53d1aaa69eb0
```

Parent inmediato:

```text
96b95172bae389f117c3c7e2afed7844eb79e98d
```

Tree:

```text
24efaa448bf4cd0ac6f7c488c9dd01357d91ad0d
```

Git permanece SOLO LECTURA para el asistente.

## Estado del frente

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-CAPABILITY-EXTRACTION
CLOSED / VERIFIED / CURRENT

PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
CLOSED / VERIFIED / CURRENT

USERS-PERSISTED-DATA-CUTOVER
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROFILES-CONFIGURATION
CLOSED / VERIFIED / CURRENT

NONPROMOTED-ACCESS-SEMANTICS-CORRECTION
CLOSED / VERIFIED / CURRENT

NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
PLANNED / NEXT

USERS-ADMINISTRATION-SURFACE-CUTOVER
PLANNED
```

Quedan SUPERSEDED:

```text
USERS-PROFILES-SOURCE-OWNERSHIP-CUTOVER
USERS-PROFILES-COMPOSITION-CUTOVER as previous Source-centric model
ACCESS-PROFILES-CONFIGURATION as generic Atlanticus Access target
USER_NOT_PROMOTED -> 403
```

## Regla principal

Atlanticus es una base generic reusable.

Global Users no conoce aplicaciones concretas.

```text
Global Users
    identity + lifecycle + global base authority
```

Profiles es generic y reusable:

```text
Profiles
    profile definition + catalog + configuration + Source lifecycle
```

Access pertenece a la aplicación cuando sus permisos son application-specific:

```text
ADA
    user -> profiles
    profile -> ADA access keys
```

Navigation es generic y consume profile keys; no debe importar Users ni ADA Access para
resolver rutas.

## Users

Users puede existir standalone.

CURRENT estructura:

```text
web/capabilities/users/
├── activity
├── blob
├── core
└── cosmos
```

No existen CURRENT:

```text
users/configuration
users/projection-cosmos
users-manager composition
```

### Strong identity

```text
issuer
subject_id
user_id = build_user_key(issuer, subject_id)
```

Email/display name no autorizan merge automático entre strong identities distintas.

### UserRecord

```text
user_id
issuer
subject_id
display_name
email
enabled
authority_key
avatar_background_color
avatar_text_color
```

No contiene:

```text
profile_key
profile_keys
access_keys
app_id
navigation role
ADA Access
Tools/KPI configuration
```

### Authorities

Managed global:

```text
basic
root
```

Runtime local:

```text
local
```

`administrator` no pertenece a Users CURRENT.

`guest` no pertenece al Users authority contract CURRENT.

No existe mapping:

```text
administrator -> root
```

### Login / access semantics

Login continúa siendo read-only sobre el promoted store:

```text
resolve only
no observe
no pending write
```

La ausencia de un promoted User no bloquea el ingreso.

CURRENT:

```text
record absent
→ AccessStatus.READY
→ deterministic user_id
→ UsersRuntime has no EffectiveUser for that load

record present + enabled=True
→ AccessStatus.READY
→ EffectiveUser stored in UsersRuntime

record present + enabled=False
→ AccessStatus.USER_DISABLED
→ 403
```

`AccessStatus.USER_NOT_PROMOTED` fue eliminado.

Promotion significa administración/control y disponibilidad de `EffectiveUser`, no
permiso básico para entrar a la aplicación.

Legacy documents `pending` / `resolved` no son aceptados por el store CURRENT.

## Users Registry / Administration

Durable contract:

```text
UsersRegistryStore
```

Provider CURRENT:

```text
BlobUsersRegistryStore
```

Default blob:

```text
users/users.json.gz
```

Documento:

```text
document_type = atlanticus_users_registry
schema_version = 1
```

Administration core continúa separado del login runtime.

No reintroducir pending writes durante login.

## Profiles

Profiles es generic Atlanticus first-class capability.

CURRENT:

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
Profiles Source lifecycle
```

`ProfileDefinition` CURRENT:

```text
key
label
background_color
text_color
```

Source contract CURRENT:

```text
document_type = atlanticus_profiles_configuration_release
schema_version = 1
resource_path = profiles/configuration.json.gz
```

Source Store es injected y generic; Profiles no hardcodea proveedor Blob/Cosmos.

No agregar permisos ADA ni un campo arbitrario `options` al modelo generic.

## ADA Access

Generic Atlanticus Access fue rechazado antes de integración por falta de evidencia de
reutilización.

El owner CURRENT es ADA:

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration
```

Contratos CURRENT:

```text
UserProfileAssignment
    user_id
    profile_keys

ProfileAccessGrant
    profile_key
    access_keys

EffectiveAdaAccess
    user_id
    profile_keys
    access_keys

AdaAccessConfiguration
    user_profiles
    profile_access
```

ADA Access valida referencias contra `ProfileCatalog` de forma explícita.

Un user_id sin assignment devuelve acceso ADA efectivo vacío; no crea asignación ni
perfil por defecto dentro de `AdaAccessConfiguration`.

Source contract CURRENT:

```text
document_type = ada_access_configuration_release
schema_version = 1
resource_path = access/configuration.json.gz
```

No existe Projection de ADA Access CURRENT.

No inventar una hasta que un consumidor real justifique esa segunda persistencia.

## Navigation

Navigation permanece generic configuration domain.

CURRENT autorización de rutas:

```text
NavigationPrincipal.access_key
        ∈
NavigationRouteMatch.allowed_profiles
```

`NavigationPrincipal.unrestricted=True` omite el filtro por profile key para rutas
habilitadas.

Navigation no necesita Users ni ADA Access para ese contrato.

### Desalineamiento pendiente

CURRENT `navigation/configuration/profiles.py` todavía define:

```text
NavigationProfileOption
_BASE_PROFILES:
    local          unrestricted
    administrator  unrestricted
    guest          restricted
```

Ese mini-modelo duplica semántica que ahora debe alinearse con Profiles y conserva
`administrator` como special-case unrestricted aunque `administrator -> root` está
prohibido.

El siguiente incremento debe resolver exclusivamente:

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
```

### Reglas ya decididas para el alignment

```text
Navigation -> Users
FORBIDDEN

Navigation -> ADA Access
FORBIDDEN

root
unrestricted para Navigation

local
unrestricted para Navigation

guest
perfil restringido normal

administrator -> root
FORBIDDEN
```

Un usuario autenticado sin promoted User puede entrar. La materialización exacta del
fallback `guest` para Navigation sigue pendiente del siguiente incremento y no debe
resolverse creando una authority `guest` en Users ni un UserRecord ficticio.

### Profiles opcional

Navigation core no debe importar lifecycle/storage de Profiles.

La integración con `ProfileCatalog` puede aportar validación o profile options en la
superficie de configuración, pero el contrato durable de Navigation conserva keys.

La conducta exacta de administración/runtime cuando Profiles no está instalado debe
ser verificada en el siguiente incremento contra el código y consumers actuales; no
inventarla desde este documento.

## Source / Projection

Source/Projection aplica por capability sólo cuando el contrato lo requiere.

CURRENT:

```text
Profiles
Source lifecycle: YES
Projection: NO

ADA Access
Source lifecycle: YES
Projection: NO

Navigation
existing generic configuration Source/Projection contract remains CURRENT

Global Users
Configuration Source: NO
Generic Projection: NO
```

No restaurar simetría artificial entre domains.

## UI

Legacy Users configuration UI permanece eliminada.

Users Administration UI sigue pendiente y debe consumir el lifecycle de administración,
no reconstruir la antigua Users Configuration.

Profiles UI debe ser Profiles-owned cuando exista.

Navigation UI permanece Navigation-owned.

ADA Access UI, si se introduce, debe ser ADA-owned.

No fusionar estas superficies sólo porque convivan en una misma aplicación.

## Persisted data

El código CURRENT no lee legacy Users schemas.

Durante el cierre de persisted-data se confirmó que no existían datos reales ya
persistidos que requirieran migración; por tanto no se creó reader legacy, migrador ni
compatibilidad temporal.

Ese frente queda CLOSED.

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
implementation internals
```

## Reglas congeladas

```text
Atlanticus generic
REQUIRED

Global Users standalone
REQUIRED

Global Users app-specific state
FORBIDDEN

Global User strong identity
issuer + subject_id

Managed authority
basic | root

local
LOCAL-RUNTIME ONLY

administrator
REMOVED FROM USERS

guest
REMOVED FROM USERS AUTHORITY CONTRACT

Users Configuration Source
REMOVED

Users generic Projection
REMOVED

Users Manager module
REMOVED

Users login write/pending
FORBIDDEN

not promoted -> 403
REMOVED

promoted disabled -> 403
CURRENT

OLD SCHEMA RUNTIME READERS
FORBIDDEN

ADAPTERS / SHIMS / ALIASES
FORBIDDEN

DOUBLE CONTRACT
FORBIDDEN

Profiles
GENERIC ATLANTICUS FIRST-CLASS CAPABILITY

Generic Access
NOT ADOPTED

ADA Access
APPLICATION-SPECIFIC / CURRENT

Navigation authorization input
PROFILE KEY

Navigation dependency on Users
FORBIDDEN

Navigation dependency on ADA Access
FORBIDDEN
```

## Orden de implementación refinado

```text
1. USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
   CLOSED / VERIFIED / CURRENT

2. USERS-PERSISTED-DATA-CUTOVER
   CLOSED / VERIFIED / CURRENT

3. PROFILES-CAPABILITY-EXTRACTION
   CLOSED / VERIFIED / CURRENT

4. PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
   CLOSED / VERIFIED / CURRENT

5. ADA-ACCESS-PROFILES-CONFIGURATION
   CLOSED / VERIFIED / CURRENT

6. NONPROMOTED-ACCESS-SEMANTICS-CORRECTION
   CLOSED / VERIFIED / CURRENT

7. NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
   PLANNED / NEXT
```

Users Administration surface permanece pendiente, pero no debe mezclarse en el punto 7.

## Pendientes explícitos

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
PLANNED / NEXT

exact guest fallback composition for authenticated non-promoted identities
OPEN / NEXT BOUNDARY

Navigation behavior when Profiles is not installed
OPEN / MUST VERIFY AGAINST CURRENT CONSUMERS

Users Administration UI/repair
PLANNED / SEPARATE

Manager authorization stale administrator/local semantics
OPEN / SEPARATE

concrete Entra/Graph Directory provider
UNVERIFIED

local selector composition wiring
UNVERIFIED

Python 3.14.7 metadata alignment
OPEN / SEPARATE

WEB-TEST-CONTRACT-CLEANUP
OPEN / SEPARATE
```
