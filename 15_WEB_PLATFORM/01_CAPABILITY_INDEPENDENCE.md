# Web Platform — Capability Independence

Estado: **CURRENT / REFINED AFTER NAVIGATION-PROFILES ALIGNMENT**

## Regla

Independencia técnica de una capability no significa que toda combinación de
capabilities sea válida operacionalmente.

Separar:

```text
core dependency
```

de:

```text
composition requirement
```

Las integrations deben permanecer en composition/binding cuando no exista una
responsabilidad de dominio que justifique acoplar cores.

## Global Users

Users puede existir standalone y es independiente de una aplicación concreta.

```text
Global Users
VALID
```

Users CURRENT no es configuration Source.

Estructura:

```text
users/core
users/blob
users/cosmos
users/activity
```

Global User no contiene:

```text
profile_key
profile_keys
access_keys
app role
Navigation configuration
Tools configuration
KPI configuration
ADA-specific Access
```

Strong identity:

```text
issuer + subject_id
```

## Profiles

Profiles es first-class capability generic Atlanticus.

CURRENT:

```text
profiles/core
profiles/configuration
```

`profiles/core` posee `ProfileDefinition` y `ProfileCatalog`.

`profiles/configuration` posee `ProfilesConfiguration` y su Source lifecycle.

Una aplicación puede asociar Profiles a Global Users sin transferir ownership del Users
registry hacia Profiles y sin agregar estado application-specific al Global User.

## Access

Access es application-specific cuando sus permisos pertenecen a una aplicación.

ADA Access CURRENT vive bajo `scopes/ada`.

No agregar Access al Global `UserRecord`.

No convertir ADA Access en dependency de Navigation core/configuration.

## Navigation

Navigation continúa siendo configuration domain generic.

Estado:

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

Boundary CURRENT:

```text
Navigation core
independiente de Users / Profiles lifecycle / ADA Access

Navigation Configuration
    -> Profiles core
       ProfileCatalog / ProfileDefinition

Navigation durable authorization
    -> allowed_profiles = profile keys
```

La integración se provee por composition mediante:

```text
NavigationProfileCatalogProvider
```

Navigation Configuration no depende de `profiles/configuration`.

No existe catálogo paralelo local de Profiles.

Removed:

```text
NavigationProfileOption
_BASE_PROFILES
NavigationProfileOptionsProvider
profile_options_provider
```

Sin `ProfileCatalog` provider, Navigation no inventa perfiles de administración.

Con provider, consume directamente `ProfileCatalog.all()` y valida referencias mediante
`ProfileCatalog.require(...)`.

Fallos del provider se propagan.

## Runtime authorization composition

La composición exacta de `NavigationPrincipal` para identidad autenticada no promovida
sigue OPEN / SEPARATE.

No resolver ese punto agregando dependencias directas entre Navigation y Users/ADA Access.

## User Activity

User Activity conserva independencia funcional respecto de Users Administration,
Navigation y Manager salvo integrations explícitas.

Su dependencia mínima puede seguir siendo:

```text
Identity
+
Web runtime
```

Un binding de Navigation hacia Activity puede existir sin fusionar sus domains.

## Manager

Manager registra únicamente módulos de Configuration presentes en la composition.

No obliga por sí mismo a instalar:

```text
Users
Profiles
Navigation
Tools
KPI
Alarm
...
```

Users CURRENT no es `ManagerModule`.

Cada módulo administrativo conserva ownership propio.

La autorización interna de Manager mantiene un gap stale de `is_local`/`administrator`;
es un frente separado.

## Invariante estructural

Cuando varias capabilities tienen la misma responsabilidad, usar el mismo concepto.

Para configuration domains que tengan ambas responsabilidades:

```text
<capability>/core
<capability>/configuration
```

No aplicar este patrón mecánicamente a Users: `users/configuration` fue eliminado porque
la responsabilidad no corresponde.

Para Profiles CURRENT:

```text
profiles/core
profiles/configuration
```

`profiles/management` no es parte del target.

## Dashboard

Dashboard puede unificar visualmente información de varias capabilities sin convertir
esa vista en dependencia de dominio.

```text
Users data ─────┐
Activity data ──┼──► Dashboard/read model
Navigation ─────┘
```

Los productores preservan ownership.

## Estado de implementación

En:

```text
moragaga/atlanticus@3eb46dac80f23d438774e3afa39999dc96f592d7
```

están CLOSED / VERIFIED / CURRENT:

```text
USERS-GLOBAL-REGISTRY-ROOT-CUTOVER
USERS-PERSISTED-DATA-CUTOVER
PROFILES-CONFIGURATION-BOUNDARY-CUTOVER
PROFILES-CAPABILITY-EXTRACTION
PROFILES-INDEPENDENT-SOURCE-LIFECYCLE
ADA-ACCESS-PROFILES-CONFIGURATION
NONPROMOTED-ACCESS-SEMANTICS-CORRECTION
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
```

Siguiente gap recomendado para debate separado:

```text
Manager authorization stale administrator/local semantics
OPEN / PROPOSED NEXT
```
