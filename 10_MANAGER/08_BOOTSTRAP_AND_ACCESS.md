# Manager — Bootstrap and Access

Estado: **CURRENT / ADA ACCESS INTEGRATED / NAVIGATION + ACCESS + PROFILES UI CLOSED**

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
access.manage
navigation.manage
tools.manage
kpis.manage
```

Profiles usa `ManagerAuthorizationPolicy.can_view(...)` mediante su composition reusable.

Users usa la misma semántica mediante `web/compositions/users-manager`.

Access conserva su context application-specific y exige explícitamente `access.manage`.

No usan Profiles ni `is_local` como privilegios implícitos de Manager.

## Local runtime

CURRENT:

```text
ManagerPrincipal(
    subject_id='local',
    display_name='Administrador local',
    access_keys=(
        users.manage,
        profiles.manage,
        access.manage,
        navigation.manage,
        tools.manage,
        kpis.manage,
    ),
    is_local=True,
)
```

`is_local=True` conserva contexto de ejecución local; no reemplaza `access_keys`.

## Functional permission boundary

Manager permission responde a:

> ¿puede este principal administrar esta capability/item?

No responde a permisos internos específicos de cada workflow.

`ManagerEntry` puede tener lifecycle de dominio propio, como Users Administration.

## ADA Access CURRENT

Las `access_keys` de `ManagerPrincipal` son inputs de authorization del Manager.

El dominio ADA Access modela por separado:

```text
AdaAccessConfiguration
├── access_keys
└── profile_access
```

Semántica de Profiles en ADA Access:

```text
root / local
→ unrestricted
→ todos los access_keys definidos
→ no explicit grants

basic / guest / custom
→ explicit grants
```

## Profiles UI localization CURRENT

ADA Configuration Manager compone la capability generic Profiles con:

```text
title='Perfiles'
description='Define los perfiles disponibles y su presentación visual dentro del sistema.'
source_name='Local Source'
projection_name='In-process Projection'
```

Esto es application-specific.

Defaults generic de `compose_profiles_manager`:

```text
title='Profiles'
description=''
source_name='Profiles Source'
projection_name='Profiles Projection'
```

La UI de Profiles está cerrada y aceptada manualmente en `df5b995...`.

## Finding CURRENT

`web/compositions/navigation-manager` conserva un consumer desalineado respecto de
`ManagerAuthorizationPolicy.can_view(...)`.

Estado:

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

No introducir alias.

## Siguiente frontera

```text
USERS-MANAGER-UI-REVIEW
PLANNED / NEXT
```

Users debe conservar su forma de `ManagerEntry` y su `UsersAdministrationService` existente.

No abrir durante ese foco:

```text
Herramienta en paralelo
ADA Access runtime composition
Manager real persistence qualification
Navigation authorization alignment
global cleanup
```
