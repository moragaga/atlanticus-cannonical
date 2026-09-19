# ADA Command Center — Identity, Users, Profiles, Access, Navigation and Activity

Estado: **CURRENT DIRECTION / REFINED AFTER NAVIGATION-PROFILES ALIGNMENT**

## Identity

Producción usa Microsoft Entra ID mediante la capability transversal Atlanticus.

No crear autenticación paralela.

## Entrada a la aplicación

La identidad autenticada puede entrar aunque todavía no exista un `UserRecord`
promovido.

Contrato CURRENT:

```text
valid authenticated identity + no promoted UserRecord
→ READY
→ deterministic user_id
→ no UsersRuntime user

promoted + enabled=True
→ READY
→ EffectiveUser available

promoted + enabled=False
→ USER_DISABLED
→ 403
```

`USER_NOT_PROMOTED` no forma parte del contrato CURRENT de Identity Access.

La promoción existe para administrar/controlar el usuario y asociarlo con estado de
aplicación; no para habilitar la autenticación base.

## Capability independence

Command Center puede integrar de forma independiente:

```text
Users
Profiles
ADA Access
Navigation
User Activity
```

No establecer:

```text
Navigation requires Users
Navigation requires ADA Access
Activity requires Navigation
Users requires Activity
Atlanticus Profiles requires ADA Access
```

## Users

Users es generic Atlanticus y no contiene estado ADA-specific.

Global `UserRecord` no incluye:

```text
profile_key
profile_keys
access_keys
navigation role
ADA state
```

Managed global authorities CURRENT:

```text
basic
root
```

Runtime local authority:

```text
local
```

`guest` y `administrator` no pertenecen al authority contract de Users.

## Profiles

Profiles es capability generic Atlanticus first-class.

CURRENT domain:

```text
ProfileDefinition
ProfileCatalog
ProfilesConfiguration
Profiles Source lifecycle
```

`ProfileDefinition` contiene:

```text
key
label
background_color
text_color
```

No agregar opciones/permisos ADA arbitrarios al modelo generic Profiles.

## ADA Access

ADA Access es application-specific y CURRENT bajo:

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration
```

Contratos principales:

```text
UserProfileAssignment
ProfileAccessGrant
EffectiveAdaAccess
AdaAccessConfiguration
AdaAccessSourceService
```

Ownership:

```text
Global user_id -> ADA profile_keys
ADA profile_key -> ADA access_keys
```

ADA Access consume `ProfileCatalog` para validación explícita.

No modificar `UserRecord` para almacenar Profiles o Access.

## Navigation

Navigation es capability generic y permanece independiente de Users y ADA Access.

Autorización CURRENT de rutas usa:

```text
NavigationPrincipal.access_key
        ∈
allowed_profiles
```

más `principal.unrestricted` cuando corresponde.

Navigation configura perfiles por key; no persiste copias de `ProfileDefinition`.

### Navigation / Profiles alignment

Estado:

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

Navigation Configuration consume `ProfileCatalog` desde Profiles core mediante:

```text
NavigationProfileCatalogProvider
```

Sin provider configurado no inventa perfiles base.

Con provider configurado:

```text
ProfileCatalog.all()
→ perfiles disponibles en UI administrativa

ProfileCatalog.require(profile_key)
→ validación referencial de allowed_profiles
```

Unknown profile produce issue:

```text
navigation.profile.unknown
```

El mismo validator referencial participa en draft validation y Projection dentro de
Navigation Manager.

Fallos del provider se propagan.

### Removed legacy

Ya no existen como contrato Navigation Configuration:

```text
NavigationProfileOption
_BASE_PROFILES
local/administrator/guest mini-catalog
NavigationProfileOptionsProvider
profile_options_provider
```

`administrator` no recibe semántica unrestricted especial desde Navigation Configuration.

No existe mapping:

```text
administrator -> root
```

`local` tampoco se representa como `ProfileDefinition` especial.

### Runtime fallback pendiente

Un usuario Entra autenticado sin promoted `UserRecord` puede entrar en la aplicación.

La materialización exacta de su `NavigationPrincipal`/fallback `guest` sigue fuera de
este hito.

No crear:

```text
Global User ficticio
guest authority en Users
Navigation -> Users dependency
Navigation -> ADA Access dependency
```

Estado:

```text
OPEN / SEPARATE
```

## Manager authorization

La semántica stale de `is_local` / `administrator` dentro de Manager sigue siendo un
frente separado.

No confundirla con Navigation authorization ni con Profiles.

## User Activity

User Activity sigue siendo opcional e integrable.

No depende obligatoriamente de Navigation y no es requisito para Alarm Engine ni
Analytics.

## Reglas congeladas

```text
Entra valid identity without promotion
ACCESS ALLOWED

promoted disabled User
ACCESS BLOCKED

Users -> app-specific Profiles/Access fields
FORBIDDEN

Navigation -> Users dependency
FORBIDDEN

Navigation -> ADA Access dependency
FORBIDDEN

Navigation Configuration -> Profiles core
CURRENT

Navigation Configuration -> Profiles Configuration
FORBIDDEN

Generic Atlanticus Access capability
NOT ADOPTED

ADA Access
APPLICATION-SPECIFIC

Profiles
GENERIC ATLANTICUS

Navigation durable profile references
PROFILE KEYS
```
