# Manager — Navigation and Home

Estado: **CURRENT / NAVIGATION CONFIGURATION CLOSED / ADA LOCAL FLOW QUALIFIED**

Última verificación de implementación para este delta: `moragaga/atlanticus@a6061ffed59c8b04e64b0a7fdc17050ef463c850`.

## `/manager` y Registry

`/manager` es una Home real. No corresponde al primer módulo ni redirige implícitamente.
`ManagerModuleRegistry` posee `modules`, `entries` e `items = modules + entries`.
Home, sidebar y routing derivan de ese mismo registry. Un ítem sólo se representa cuando
`visible_items(principal, policy)` autoriza su visibilidad.

## Fronteras de navegación

- Manager posee su botón lateral, Home, grupos, módulos, rutas administrativas y sidebar.
- Navigation Core/Configuration posee rutas operacionales configurables y su autorización.
- ADA Generic integra ambos sin fusionar la navegación administrativa con el menú operacional.
- Las capacidades administrativas conservan sus propias claves Manager, por ejemplo
  `navigation.manage`. La excepción de recuperación Navigation **no concede** por sí misma
  permisos de otros módulos Manager.

## Grupos CURRENT de ADA Configuration Manager

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

La superficie administrativa pertenece a Navigation Configuration y no depende físicamente de
Profiles. Los proveedores opcionales son `NavigationProfileOption` y
`NavigationProfileOptionsProvider`; ADA adapta Profiles a esas opciones.

Paginación top-level: default `10`, valores `10/20`; mezcla root links y sections en orden
durable. Una section es un ítem; sus hijos no cuentan en el total. Estado expandido efímero.
El empty state conserva su área; corregir geometría, no esconder overflow como solución.

## Qualification local adicional — 2026-09-25

**VERIFIED MANUAL / CLOSED en el entorno local del usuario:** ADA Generic arranca,
Navigation permite guardar/publicar/proyectar, y la página Home consume el menú configurado.
Tras una regresión posterior del callback, el usuario confirmó que el menú funciona sobre
`a6061ffe`.

**VERIFIED STATIC:** `a6061ffe` contiene controller fuera del Offcanvas, ruta previa en Store,
distinción entre clic y cambio efectivo de pathname, y triggers sin atributo `title` de tooltip,
con texto oculto accesible. No deducir ejecución de tests finales a partir de esa inspección.

**UNVERIFIED:** clic móvil/escritorio bajo matriz de navegadores; persistencia real en
Blob/Cosmos después de reiniciar; identidad Entra en un host productivo.

## Estado

```text
NAVIGATION CONFIGURATION ADMIN UI           CLOSED / CURRENT
ADA GENERIC NAVIGATION LOCAL FLOW           CLOSED / VERIFIED MANUAL / CURRENT
NAVIGATION CLIENT CODE CORRECTION           CURRENT / USER-REPORTED FUNCTIONAL
MANAGER REAL DURABLE PERSISTENCE            PLANNED / UNVERIFIED
```

El Manager genérico no cambia de contrato por esta qualification local.
