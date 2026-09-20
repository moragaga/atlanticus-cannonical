# Manager — Bootstrap and Access

Estado: **CURRENT / ADA ACCESS INTEGRATED / ACCESS UI CLOSED**

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

Profiles usa la misma semántica `ManagerAuthorizationPolicy.can_view(...)` mediante su
composition reusable.

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

No derivar permisos Manager desde Profiles o ADA Access sin requisito explícito de composición
de producto.

## ADA Access CURRENT

Las `access_keys` de `ManagerPrincipal` son inputs de authorization del Manager.

El dominio ADA Access modela por separado:

```text
AdaAccessConfiguration
├── access_keys
└── profile_access
```

No se unifican automáticamente ambos contracts.

Semántica de Profiles en ADA Access:

```text
root
→ unrestricted
→ todos los access_keys definidos
→ no explicit grants

local
→ unrestricted
→ todos los access_keys definidos
→ no explicit grants

basic / guest / custom
→ explicit grants
```

La identidad durable sigue siendo un único string `access_key`.

La UI de creación usa `Ámbito + Permiso` para componer `ámbito.permiso`, sin crear un segundo
schema durable.

## ADA Access Manager surface CURRENT

```text
Accesos
├── access catalog
└── pagination 10 / 20

Perfiles
├── basic / guest / custom assignable
├── root / local excluded
├── compact assignment summary
└── Configurar -> modal
```

Si no existe Profiles Projection activa, la UI puede mostrar el catálogo de sistema
`ProfileCatalog()`; al excluir `root` y `local`, quedan `basic` y `guest`.

La validación de draft conserva la exigencia de Profiles Projection activa.

El modal:

- no abre sin access keys;
- se centra respecto del viewport;
- usa `dbc.Checkbox`;
- escribe la asignación confirmada directamente en la configuración editable.

No existe un overlay durable ni un segundo contrato de asignaciones.

La paginación reutiliza `atlanticus.web.pagination`.

El overflow horizontal se corrige en su causa local; no se oculta globalmente.

## UI localization CURRENT

ADA Configuration Manager compone la capability generic Profiles con:

```text
title='Perfiles'
```

Esto es application-specific.

El default generic de `compose_profiles_manager` sigue siendo `Profiles`.

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

Access UI está cerrada.

El siguiente foco único acordado es:

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS / NEXT PAGE: PERFILES
```

No abrir durante ese foco:

```text
ADA Access runtime composition
Manager real persistence qualification
Navigation authorization alignment
global cleanup
```
