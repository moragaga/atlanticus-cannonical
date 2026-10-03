# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global — FROZEN

```text
uv; no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
one focus per increment
Git read-only unless explicit authorization
```

## Users model — CURRENT / CLOSED

```text
UserIdentity                   application-global
ToolUserMembership             Tool-scoped
RuntimeUser                    Tool runtime snapshot
```

SUPERSEDED:

```text
global UserRecord owns profile_key + enabled
```

## Command Center capability parity — CURRENT / CLOSED

Command Center usa el mismo contrato genérico de capabilities que ADA para:

```text
Users
Profiles
Navigation
Manager
```

Esto significa:

```text
UsersAdministrationService(registry, memberships, profiles, directory)
UsersRegistryStore
ToolMembershipStore
UsersRuntimeStore
generic Profiles composition
generic NAVIGATION_SOURCE_KEY
RuntimeUser -> ManagerPrincipal binding
```

No mantener API legacy ni aliases de compatibilidad.

## Navigation authorization — CURRENT / CLOSED en capabilities

```text
PUBLIC
RESTRICTED + []
RESTRICTED + [profiles]
```

`allowed_profiles=[]` ya no equivale por sí solo a acceso público.

Root/local override pertenece a composición de principal/autorización y no se persiste como grant ordinario.

## Users recovery — CURRENT / CLOSED

```text
Global Users
+ Tool Membership
+ Profiles
+ Operational
→ RuntimeUser[]
```

Recovery reemplaza runtime completo y no muta Registry ni Membership.

Users sigue siendo operación especial en Master Projection, no Source Projection ordinaria.

## Command Center qualifier — BLOCKED separado

El drift de Users/Profiles/Navigation/Manager está cerrado.

El qualifier completo permanece bloqueado por la coexistencia de contratos Tools:

```text
ada.web.tools.*
ada.contracts.tools.*
```

No resolver este conflicto modificando Command Center ni ADA dentro de este cierre.

## Alarm Engine ownership — REFINED / PLANNED

CURRENT físico:

```text
scopes/ada-command-center/backend
```

Dirección propuesta para el próximo frente:

```text
Command Center
    authoring
    semantic validation
    Tool/reference validation
    publication
        ↓
shared published Alarm contract
        ↓
ADA Alarm Engine
    core
    materialization
    persistence
    runtime
    delivery
```

Working hypothesis:

```text
todo el backend de Alarmas actual pertenece conceptualmente al Engine;
las dependencias hacia Web son deuda de frontera que debe removerse o invertirse.
```

Esto aún no declara la extracción implementada.

## No ADA durante este cierre

La incompatibilidad Tools observada queda registrada como bloqueo separado.

No modificar:

```text
scopes/ada/web
```

hasta abrir explícitamente ese frente.

## Next único

```text
ADA-ALARM-ENGINE-EXTRACTION-DESIGN
```

Primero inventario y contratos; luego implementación incremental tras consenso.
