# Manager — Navigation and Home

Estado: **CURRENT / MODULES + ENTRIES / NAVIGATION CONFIGURATION SLICE CLOSED**

## `/manager`

`/manager` es una Home real.

No representa el primer item administrativo y no redirige implícitamente a un módulo.

## Registry

`ManagerModuleRegistry` es la fuente única de items administrativos visibles y rutas.

Mantiene:

```text
modules
entries
items = modules + entries
```

Home, sidebar y routing derivan del mismo registry.

## Visibilidad

```text
ManagerModuleRegistry
        ↓
visible_items(principal, policy)
        ↓
render Home / sidebar
```

No renderizar un item antes de comprobar autorización.

## Navegación Manager vs Navigation capability

Manager conserva navegación administrativa propia:

- botón/trigger lateral;
- Home;
- grupos;
- items administrativos;
- retorno explícito;
- sidebar.

Navigation capability mantiene navegación operacional/configurable.

No fusionar ambos ownerships.

## Grupos CURRENT en ADA Configuration Manager

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

## Navigation Configuration CURRENT

La surface administrativa de Navigation es propiedad de Navigation Configuration.

No depende físicamente de Profiles.

Contrato opcional para opciones de acceso:

```text
NavigationProfileOption
NavigationProfileOptionsProvider
```

ADA composition adapta Profiles a este contrato.

## Navigation top-level pagination

CURRENT:

```text
DEFAULT
10

ALLOWED
10 / 20
```

La colección paginada mezcla los nodos top-level según el orden durable:

```text
root links
sections
```

Cada section cuenta como un item.

Los child links no cuentan para el total de página.

Una section expandida muestra todos sus hijos y puede hacer crecer la página sobre su
min-height visual.

El estado expandido es efímero.

## Empty state y overflow

Cuando no existen nodos, Navigation conserva la superficie reservada para la página y centra
el empty state.

El control Dash de page size contiene su `dash-dropdown-focus-target` dentro del wrapper para
evitar overflow horizontal.

No usar `overflow-x: hidden` como sustituto de corregir geometría defectuosa.

## Manager UI review

Navigation:

```text
CLOSED / VERIFIED MANUAL / CURRENT
```

Overall:

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Siguiente página:

```text
Herramienta
```

## Principio

La navegación Manager organiza capabilities administrativas.

Navigation organiza la navegación operacional/configurable.

Son responsabilidades distintas aunque convivan en ADA Configuration Manager.
