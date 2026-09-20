# Manager — Canonical Index

Estado: **CURRENT GENERIC CORE / PROFILES + USERS ADMIN INTEGRATED / ADA ACCESS NEXT**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como capability independiente. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar y navegación administrativa sobre items registrados. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION para módulos y frontera de entries administrativos. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Tool Configuration y contrato Source/Projection. | FROZEN/CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection consumido por Manager genérico. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuentes/checkpoints/evidencia. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Bootstrap separado de Manager Access. | CURRENT |
| `09_ADA_COMPONENT_LINKS.md` | Links externos y warmup. | CONTRACT DESIGN |

## Autoridad de implementación verificada

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

El parent `e0dca2d9...` contiene la implementación funcional Users Manager.
El checkpoint CURRENT `783d3578...` elimina únicamente el `uv.lock` anidado accidental de
`web/compositions/users-manager`; la autoridad de lock del workspace Web permanece en
`web/uv.lock`.

## Contratos Manager CURRENT

### ManagerModule

`ManagerModule` representa una capability administrativa respaldada por Source/Projection:

```text
ManagerModule
├── key
├── group_key
├── title
├── route
├── order
├── layout
├── source_key
├── source_service
├── source_reader_service
├── projection_service
├── draft_validation_service
├── source_history_service | None
├── access_key | None
└── web_module | None
```

### ManagerEntry

`ManagerEntry` representa una capability administrativa visible en el mismo shell Manager
sin exigir un lifecycle Source/Projection ficticio:

```text
ManagerEntry
├── key
├── group_key
├── title
├── route
├── order
├── layout
├── description
├── access_key | None
└── web_module | None
```

`ManagerModule` y `ManagerEntry` comparten navegación, routing, authorization y lifecycle
de `WebModule`, pero sólo `ManagerModule` participa del coordinator Source/Projection.

No existe dual contract legacy/exact.

## Registry CURRENT

`ManagerModuleRegistry` mantiene separadas:

```text
modules
entries
```

y expone la vista combinada:

```text
items
```

Invariantes:

- key y route son únicos en el conjunto combinado;
- `require()` resuelve sólo `ManagerModule`;
- `require_entry()` resuelve sólo `ManagerEntry`;
- `visible_modules`, `visible_entries` y `visible_items` aplican la misma policy;
- Home, sidebar y routing derivan del mismo registry;
- el coordinator Source/Projection continúa operando sólo sobre `ManagerModule`.

## Authorization CURRENT

```text
ManagerAuthorizationPolicy.can_view(principal, item)
```

donde `item` puede ser:

```text
ManagerModule | ManagerEntry
```

No existe bypass por:

```text
principal.is_local
profile administrator
```

Si `access_key` es `None`, la policy default deniega acceso.

## Profiles Manager CURRENT

Existe:

```text
web/compositions/profiles-manager
```

Estado:

```text
PROFILES-MANAGER-COMPOSITION
CLOSED / VERIFIED / CURRENT

PROFILES-ADA-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT
```

ADA Configuration Manager consume el `ManagerModule` producido por la composition existente.

Capability explícita:

```text
profiles.manage
```

## Users Manager CURRENT

Users no es `ManagerModule` Source/Projection.

Existe:

```text
web/compositions/users-manager
```

La composition recibe un `UsersAdministrationService` ya construido y produce:

```text
ManagerEntry
```

Capability explícita:

```text
users.manage
```

Ruta CURRENT dentro de ADA Configuration Manager:

```text
/manager/users
```

Estado:

```text
USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT
```

No crear para Users:

```text
Source ficticio
Projection ficticia
ManagerModule Source/Projection
adapter
shim
alias
segundo lifecycle administrativo
```

## ADA Access CURRENT

ADA Access es application-specific y ya posee Source/Projection CURRENT.

Su Projection dispone de persistencia durable local y Cosmos preservando
`ProjectionTarget` y dependencies exactas.

Estado:

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT
```

No existe una Web/UI administrativa de ADA Access en la implementación CURRENT.

La historia inspeccionada de `scopes/ada/web/access` desde su introducción contiene core,
configuration, Source/Projection y stores local/Cosmos, pero no una Web surface de Access.

Siguiente frontera:

```text
ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
PLANNED / NEXT / DESIGN FIRST
```

El próximo diseño debe partir del contrato CURRENT:

```text
profile_key -> access_keys
```

y del requerimiento de producto fijado para el siguiente incremento:

```text
crear/definir accesos de forma controlada
asignar accesos definidos a perfiles
obtener/usar un identificador estable de acceso para consumo manual por desarrolladores
```

La forma exacta de ese identificador, catálogo, lifecycle y UI permanece OPEN y debe
derivarse del código CURRENT y de consumidores reales. No inventarla antes de diseño.

La integración del desarrollador es manual/controlada: no se requiere autodescubrimiento
ni modificación automática de funcionalidades Web.

## Finding separado

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`web/compositions/navigation-manager` conserva un consumer desalineado respecto de
`ManagerAuthorizationPolicy.can_view(...)`.

No añadir shim/alias.

## Secuencia congelada de continuación

```text
1. ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
   PLANNED / NEXT / DESIGN FIRST

2. MANAGER-FINAL-ADMIN-COMPOSITION
   PLANNED / AFTER ADA ACCESS
```

No mezclar en esos incrementos:

```text
ADA Access runtime composition
Navigation operational authorization alignment
Navigation disabled-route surface
concrete Entra/Graph provider
Python metadata alignment
global CI/test cleanup
```
