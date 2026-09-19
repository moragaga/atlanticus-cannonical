# Manager — Bootstrap and Access

Estado: **CURRENT / AUTHORIZATION SEMANTICS ALIGNED**

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
ManagerAuthorizationPolicy
```

No existe `ManagerModuleAccess` CURRENT.

`DefaultManagerAuthorizationPolicy`:

```text
required = module.access_key
required is None -> deny
required in principal.access_keys -> allow
otherwise -> deny
```

No existen bypass CURRENT por:

```text
principal.is_local
'administrator' in principal.profile_keys
```

## ADA Configuration Manager CURRENT

Capabilities funcionales:

```text
navigation.manage
tools.manage
kpis.manage
```

Los contexts específicos usan la misma semántica explícita:

```text
access_key in principal.access_keys
```

No usan Profiles ni `is_local` como privilegios.

## Local runtime

CURRENT:

```text
ManagerPrincipal(
    subject_id='local',
    display_name='Administrador local',
    access_keys=(
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

> ¿puede este principal administrar esta función/módulo?

No responde a:

> ¿puede ejecutar específicamente validate vs publish vs project?

Los pasos Source/Projection siguen siendo mecanismos internos del workflow del módulo.

No derivar permisos de Manager desde Profiles o ADA Access sin requisito explícito de
composición de producto.

## Finding CURRENT

`web/compositions/navigation-manager` llama `can_access(...)`, pero el protocolo actual
declara `can_view(...)`.

No introducir alias. Alinear directamente cuando ese consumer entre al scope.

## Siguiente frontera

```text
CONFIGURATION-UI-COMPOSITION-RECOVERY
PLANNED / NEXT
```
