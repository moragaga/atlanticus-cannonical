# Manager — Canonical Index

Estado: **CURRENT GENERIC CORE / FINAL ADMIN COMPOSITION INTEGRATED / UI REVIEW NEXT**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como capability independiente. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar y navegación administrativa sobre items registrados. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION para módulos y frontera de entries administrativos. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Tool Configuration y contrato Source/Projection. | FROZEN/CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection consumido por Manager genérico. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual y frontera visual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuentes/checkpoints/evidencia. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Bootstrap separado de Manager Access. | CURRENT |
| `09_ADA_COMPONENT_LINKS.md` | Links externos y warmup. | CONTRACT DESIGN |

## Autoridad de implementación verificada

```text
moragaga/atlanticus@6032cf84e8a5ad1f7a4cde4333513a04bcdd659a
```

Parent inmediato verificado:

```text
783d3578da52aeb5cf831999a7717dc8b79f2fb0
```

El checkpoint CURRENT está un commit por delante de `783d3578...`.

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

`ManagerModule` y `ManagerEntry` comparten navegación, routing, authorization y lifecycle de
`WebModule`, pero sólo `ManagerModule` participa del coordinator Source/Projection.

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

Ruta CURRENT:

```text
/manager/users
```

Estado:

```text
USERS-ADMINISTRATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT

USERS-MANAGER-CHECKLIST-COMPATIBILITY
CLOSED / VERIFIED / CURRENT
```

El fix de compatibilidad con `dash-bootstrap-components==2.0.4` mueve el estado disabled del
`dbc.Checklist` a su option. No cambia dominio, callbacks ni persistencia.

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

ADA Access es application-specific.

Contrato CURRENT:

```text
AdaAccessConfiguration
├── access_keys
└── profile_access
```

`access_key` es la identidad estable. No existe una entidad `AccessDefinition` separada ni
un identificador paralelo.

Source/Projection:

```text
ADA_ACCESS_SOURCE_SCHEMA_VERSION = 3
ADA_ACCESS_PROJECTION_SCHEMA_VERSION = 2
```

Stores local y Cosmos preservan `ProjectionTarget` y dependencies exactas.

Estado:

```text
ADA-ACCESS-PROJECTION-PERSISTENCE
CLOSED / VERIFIED / CURRENT

ADA-ACCESS-CONFIGURATION-MANAGER-INTEGRATION
CLOSED / VERIFIED / CURRENT
```

Web surface CURRENT:

```text
scopes/ada/web/access/configuration/.../web
```

Manager contract:

```text
ManagerModule
key = access
title = Accesos
route = /access
effective route = /manager/access
order = 15
access_key = access.manage
```

La UI permite:

```text
definir access keys
eliminar access keys no asignadas
asignar access keys a Profiles proyectados
guardar el payload en el workspace Manager
```

La validation del draft exige Profiles Projection disponible y valida las profile keys
contra su `ProfileCatalog`.

No existe autodescubrimiento de permisos ni asociación automática con features Web.

## Final admin composition CURRENT

ADA Configuration Manager compone:

```text
Administración
└── Users

Configuraciones
├── Profiles
├── Accesos
├── Navegación
├── Herramienta
├── KPI
└── Definiciones KPI
```

Estado:

```text
MANAGER-FINAL-ADMIN-COMPOSITION
CLOSED / VERIFIED / CURRENT

MANAGER-ALL-SURFACES-RENDERABLE
CLOSED / VERIFIED MANUAL / CURRENT
```

Durante el cierre se observó manualmente que todas las superficies son visibles/renderizables.

Ese finding no califica calidad visual ni demuestra persistencia real.

## Qualification ejecutada

```text
ADA Access Configuration
37 passed
Ruff scoped PASS
Ruff format scoped PASS

Projection Local
4 passed

Projection Cosmos
6 passed

ADA Configuration Manager
31 passed
Ruff scoped PASS
Ruff format scoped PASS
```

## Finding separado

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT
```

`web/compositions/navigation-manager` conserva un consumer desalineado respecto de
`ManagerAuthorizationPolicy.can_view(...)`.

No añadir shim/alias.

## Siguiente foco único

```text
MANAGER-UI-CONSISTENCY-REVIEW
PLANNED / NEXT
```

Debe revisar todas las superficies Manager ya compuestas y visibles, incluyendo paginación.

Durante ese incremento:

- corregir UI/responsive/spacing/overflow/paginación visual;
- validar visualmente presentación;
- preservar contratos backend y callbacks funcionales salvo defecto demostrado;
- eliminar inmediatamente tests cuyo único objetivo sea validar CSS, estilos, estructura JS,
  clases/funciones internas o estructura visual accidental;
- no crear tests nuevos para congelar markup o CSS.

Después:

```text
MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW
```

La qualification posterior comprobará comportamiento real de guardar/publicar/proyectar/
recargar.

No mezclar en UI review:

```text
ADA Access runtime authorization
Navigation operational authorization alignment
Navigation disabled-route surface
concrete Entra/Graph provider
Python metadata alignment
global CI/test cleanup no relacionado
```
