# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada publicada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.
- Git permanece SOLO LECTURA para el asistente.

## Checkpoint publicado CURRENT

```text
moragaga/atlanticus@783d3578da52aeb5cf831999a7717dc8b79f2fb0
```

Parent:

```text
e0dca2d9f9e8db9551b8cee45a37cd1ce3dd4bd5
```

Tree:

```text
5ed091d5477b8ca041ddd669de8217028ec72f35
```

El parent `e0dca2d9...` introdujo Users Manager.
`783d3578...` elimina únicamente:

```text
web/compositions/users-manager/uv.lock
```

La autoridad de lock del workspace Web es:

```text
web/uv.lock
```

## Cierres relevantes CURRENT

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
CLOSED / VERIFIED / CURRENT

PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

PROFILES-ADA-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT
```

## ManagerEntry

CURRENT añadido al core Manager:

```text
ManagerEntry
```

Campos CURRENT:

```text
key
group_key
title
route
order
layout
description
access_key
web_module
```

`ManagerModuleRegistry` conserva `modules` y añade `entries` / `items`.

El conjunto combinado exige key y route únicos.

Authorization acepta:

```text
ManagerModule | ManagerEntry
```

El coordinator Source/Projection continúa resolviendo sólo `ManagerModule`.

## Users -> ADA Configuration Manager

Implementado:

```text
web/compositions/users-manager
→ recibe UsersAdministrationService
→ produce ManagerEntry
→ registra users.administration mediante WebModule
→ usa ManagerAuthorizationPolicy.can_view(...)
```

ADA Configuration Manager CURRENT:

```text
group administration
    Users

group configuration
    Profiles
    Navigation
    Tools
    KPI
    KPI Definition
```

Capability funcional Users:

```text
users.manage
```

Ruta:

```text
/manager/users
```

No implementado:

```text
Users Source
Users Projection
Users ManagerModule Source/Projection
adapter
shim
alias
doble lifecycle
```

## Users Web surface CURRENT

Incluye:

```text
candidate/promote
promoted/edit
search/filter
conflict visibility
profile selection
enabled state
pagination 10/20
explicit Refresh
```

Sólo son administrables:

```text
profile_key
enabled
```

Read-only:

```text
user_id
issuer
subject_id
display_name
email
```

Persistencia:

```text
UsersAdministrationService only
```

Profiles options usan snapshot de page load y sólo se reemplazan mediante Refresh explícito.

## Qualification observada

En el árbol integrado antes de publicar:

```text
targeted integration                  23 passed
ADA Configuration Manager             26 passed
Users core                             46 passed
Manager                                62 passed
users-manager                           1 passed
Web scoped combined                   109 passed
git diff --check                      PASS
uv lock / uv sync                     PASS
```

No declarar:

```text
full monorepo pytest GREEN
full Ruff workspace GREEN
CI remote GREEN
Docker E2E GREEN
```

Se intentó una recolección pytest desde la raíz del monorepo y produjo errores de entornos,
dependencias y nombres de tests entre proyectos independientes. Esa ejecución no constituye
qualification válida del incremento y no se usa como evidencia de regresión Users.

## ADA Access CURRENT observado

Packages:

```text
scopes/ada/web/access/core
scopes/ada/web/access/configuration
scopes/ada/web/access/projection-local
scopes/ada/web/access/projection-cosmos
```

Ownership CURRENT:

```text
profile_key -> access_keys
```

Contracts CURRENT:

```text
ProfileAccessGrant
EffectiveAdaAccess
AdaAccessConfiguration
AdaAccessSourceService
AdaAccessProjectionBuilder
```

Source schema:

```text
2
```

Projection:

```text
exact Profiles ProjectionTarget dependency
durable ProjectionRecord[AdaAccessConfiguration]
local provider
Cosmos provider
```

No existe package/directorio Web UI de ADA Access ni dependencia Dash en estos packages.

El historial inspeccionado de commits que introdujeron y modificaron
`scopes/ada/web/access` tampoco muestra una Web surface de Access.

## Requirement fijado para la próxima frontera

El siguiente chat debe diseñar, antes de implementar, cómo representar:

```text
definición/creación controlada de accesos
asignación de accesos a Profiles
identificador estable consumible manualmente por desarrolladores
```

CURRENT sólo contiene strings `access_keys`; no existe catálogo de definiciones de acceso.

No inventar clase, schema, storage, route o nombre de identificador antes de inspeccionar
consumers reales.

La integración en funcionalidades Web será manual/controlada por desarrolladores; no se
requiere wiring automático.

## Finding pendiente separado

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

Después:

```text
MANAGER-FINAL-ADMIN-COMPOSITION
PLANNED
```
