# Manager — Bootstrap and Access

Estado: **CURRENT DIRECTION / REFINED AFTER NAVIGATION-PROFILES ALIGNMENT**

## Alcance

Este documento conserva la frontera entre Bootstrap Access y Manager Access y registra
el gap CURRENT de autorización de Manager.

El cierre `NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT` no modifica la autorización interna
de Manager.

## Bootstrap Access

Bootstrap puede existir antes de que otras capabilities de aplicación estén listas.

```text
BOOTSTRAP ACCESS
        ≠
MANAGER ACCESS
```

Producción utiliza identidad autenticada mediante el provider configurado.

Identity/Users CURRENT distingue:

```text
invalid identity
→ rejected

promoted disabled user
→ USER_DISABLED / 403

valid authenticated identity without promoted UserRecord
→ READY
```

La promoción de Users no es el gate de entrada a la aplicación.

## Manager Access CURRENT

Manager utiliza:

```text
ManagerPrincipal
ManagerModuleAccess
ManagerAuthorizationPolicy
```

Sin embargo `DefaultManagerAuthorizationPolicy` CURRENT todavía concede acceso total si:

```text
principal.is_local
OR
'administrator' in principal.profile_keys
```

antes de evaluar el access key requerido por `ManagerModuleAccess`.

Estado:

```text
VERIFIED CURRENT IMPLEMENTATION
OPEN CONTRACT CLEANUP
```

Este documento no redefine silenciosamente la política final.

## ADA Configuration Manager CURRENT

La composition ADA mantiene helpers:

```text
_can_manage_navigation
_can_manage_tools
_can_manage_kpis
```

con semántica equivalente:

```text
principal.is_local
OR
'administrator' in principal.profile_keys
OR
required access key in principal.access_keys
```

Ese duplicado pertenece al mismo frente futuro de autorización Manager.

## Local

Provider local y autoridad local son runtime concerns.

No existe mapping contractual:

```text
local -> administrator
administrator -> root
```

El siguiente incremento debe verificar cómo se otorgan permisos Manager explícitos en
runtime local antes de remover cualquier bypass.

No inventar access keys ni mappings sin inspeccionar consumers CURRENT.

## Navigation / Profiles

Estado:

```text
NAVIGATION-PROFILES-DEPENDENCY-ALIGNMENT
CLOSED / VERIFIED / CURRENT
```

Manager puede componer Navigation con un provider opcional de `ProfileCatalog`:

```text
Profiles core
    ↓
NavigationProfileCatalogProvider
    ↓
Navigation Manager draft validation + Projection validation + admin options
```

Navigation no requiere Users ni ADA Access.

No usar:

```text
Users -> Navigation profile options
ADA Access -> Navigation authorization
```

Navigation conserva sus `allowed_profiles` como referencias por key.

## Siguiente frontera recomendada

```text
Manager authorization stale administrator/local semantics
PLANNED / PROPOSED NEXT
```

Debate obligatorio antes de implementación:

1. `ManagerPrincipal` CURRENT.
2. `ManagerModuleAccess` CURRENT.
3. `DefaultManagerAuthorizationPolicy` CURRENT.
4. consumers/compositions ADA CURRENT.
5. runtime local CURRENT.
6. tests de comportamiento existentes.

No mezclar con guest fallback de Navigation, Users Administration, ADA Access runtime,
Python metadata ni cleanup transversal de tests.
