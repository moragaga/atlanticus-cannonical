# Manager — Bootstrap and Access

Estado: **CURRENT / AUTHORIZATION SEMANTICS ALIGNED / USERS + PROFILES CAPABILITIES ADDED**

## Alcance

Bootstrap Access y Manager Access son fronteras distintas.

```text
BOOTSTRAP ACCESS
        ≠
MANAGER ACCESS
```

## Bootstrap Access

Identity/Users CURRENT distingue:

```text
invalid identity
→ rejected

promoted disabled user
→ USER_DISABLED / 403

valid authenticated identity without promoted UserRecord
→ READY
```

Promotion de Users no es gate básico de entrada.

## Manager Access CURRENT

Manager usa:

```text
ManagerPrincipal
ManagerModule.access_key
ManagerEntry.access_key
ManagerAuthorizationPolicy
```

No existe `ManagerModuleAccess` CURRENT.

`DefaultManagerAuthorizationPolicy`:

```text
required = item.access_key
required is None -> deny
required in principal.access_keys -> allow
otherwise -> deny
```

`item` puede ser:

```text
ManagerModule | ManagerEntry
```

No existen bypass CURRENT por:

```text
principal.is_local
'administrator' in principal.profile_keys
```

## ADA Configuration Manager CURRENT

Capabilities funcionales:

```text
users.manage
profiles.manage
navigation.manage
tools.manage
kpis.manage
```

Profiles usa la misma semántica `ManagerAuthorizationPolicy.can_view(...)` mediante su
composition reusable.

Users usa la misma semántica mediante `web/compositions/users-manager`.

Los contexts específicos existentes conservan capacidad explícita.

No usan Profiles ni `is_local` como privilegios implícitos.

## Local runtime

CURRENT:

```text
ManagerPrincipal(
    subject_id='local',
    display_name='Administrador local',
    access_keys=(
        users.manage,
        profiles.manage,
        navigation.manage,
        tools.manage,
        kpis.manage,
    ),
    is_local=True,
)
```

`is_local=True` conserva contexto de ejecución local; no reemplaza `access_keys`.

No existe mapping contractual:

```text
local -> administrator
administrator -> root
```

## Functional permission boundary

Manager permission responde a:

> ¿puede este principal administrar esta capability/item?

No responde a:

> ¿puede ejecutar específicamente validate vs publish vs project?

Los pasos Source/Projection siguen siendo mecanismos internos de `ManagerModule`.

`ManagerEntry` puede tener lifecycle de dominio propio, como Users Administration.

No derivar permisos Manager desde Profiles o ADA Access sin requisito explícito de
composición de producto.

## ADA Access distinction

Las `access_keys` de `ManagerPrincipal` son inputs de authorization del Manager.

El dominio ADA Access CURRENT también modela:

```text
profile_key -> ADA access_keys
```

No asumir que ambos contracts deban unificarse automáticamente.

La futura integración ADA Access debe revisar consumers reales antes de decidir cómo sus
access identifiers participan en superficies Web o Manager.

## Finding CURRENT

`web/compositions/navigation-manager` conserva un consumer desalineado respecto de
`ManagerAuthorizationPolicy.can_view(...)`.

Estado:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No introducir alias.

## Próxima frontera

```text
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST
```

No existe Web surface ADA Access CURRENT.

La próxima etapa debe diseñar la creación/definición de accesos, su asignación a Profiles y
el identificador estable que el desarrollador utilizará manualmente en funcionalidades Web,
sin inventar wiring automático.

Después:

```text
MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED
```
