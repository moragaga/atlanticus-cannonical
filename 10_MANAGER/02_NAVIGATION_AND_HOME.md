# Manager — Navigation and Home

Estado: **CURRENT / LOCAL HEADER + NAVIGATION UI CLOSED / DISTRIBUTED STARTER UNVERIFIED**

Inspección de este hito: `moragaga/atlanticus@392ee281a32396516fb08c23c63514d8cbdb3489`. La cualificación previa de Navigation local permanece registrada como antecedente; no se reejecutó aquí.

## `/manager` y Registry

`/manager` es Home real, no el primer módulo ni una redirección implícita. `ManagerModuleRegistry` contiene `modules`, `entries` e `items`; Home, sidebar y routing derivan del registry y sólo representan `visible_items(principal, policy)` autorizados. El Manager tiene botón flotante, Home, sidebar y rutas administrativas propios. Navigation Core/Configuration posee rutas operacionales y autorización distinta. ADA Generic integra ambos sin fusionarlos.

Las cards de Home y el sidebar consumen el mismo registry; las cards no ejecutan workflow. Conservar la separación del documento aprobado `manager_decisions/ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md`: paginación de Home de seis cards y sidebar con filtro sobre elementos ya autorizados, sin duplicar fuentes de navegación.

## Grupos CURRENT de ADA Configuration Manager

```text
Administración
└── Usuarios

Configuraciones
├── Perfiles
├── Accesos
├── Navegación
├── Herramienta
├── KPI
└── Definiciones KPI
```

`Usuarios` se configura en la composición ADA con `title='Usuarios'`. El nombre genérico del paquete y la ruta `/manager/users` permanecen intactos. Perfiles es igualmente un label de composición ADA.

## Header CURRENT

El Manager genérico admite marcas opcionales, título y subtítulo de host y `application_home_href` opcional. No impone logos ADA. La composición ADA inyecta **ADA y Atlanticus**; eliminó del header Los Pelambres. Conserva `Manager Home`, `Volver a la aplicación` cuando aplica y el contexto de sección según ruta autorizada. **No representa el nombre del usuario en el header**; el `principal` continúa participando en autorización y resolución de rutas. No reinstalar el nombre mediante otro contenedor o CSS oculto.

La navegación operacional y el header operacional ADA siguen siendo independientes del header/sidebar Manager; no fusionarlos.

## Navigation Configuration CURRENT

Su UI pertenece a Navigation Configuration y no depende físicamente de Profiles. ADA adapta perfiles mediante `NavigationProfileOption` / provider neutral. Paginación top-level 10/20, secciones como ítems y expansión efímera. No cambiar geometría o callbacks por el cierre del header.

## Evidencia y límite

**VERIFIED STATIC:** los módulos actuales reflejan branding ADA/Atlanticus, ausencia del nombre en header, labels ADA y enlaces opcionales. **VERIFIED MANUAL:** el usuario aceptó header de ADA Generic local, enlaces, traducción y navegación de esta pantalla. **UNVERIFIED:** ese mismo recorrido en Starter ADA distribuido y persistencia durable Blob/Cosmos. No confundir prueba visual con qualification de docker/productiva.
