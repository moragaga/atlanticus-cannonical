# Manager — Application Boundary

Estado: **CURRENT / COMPOSITION CONVERGENCE PLANNED NEXT**

Implementation inspeccionada:

```text
moragaga/atlanticus@a75465745e188da4765e803595b17acaa55d9306
```

## Decisión

Manager es una capability genérica de Atlanticus.

Manager posee:

```text
surface
registry
authorization
Home
sidebar/navigation administrativa
workflow común
routing
callbacks/assets transversales
```

La presentación y reglas específicas de configuración permanecen en la capability que las posee.

Manager no es una variante del shell operacional de ADA ni del shell futuro de Command Center.

## Items administrativos

```text
ManagerModule
ManagerEntry
```

`ManagerModule` se usa cuando la capability posee un lifecycle Source/Projection real.

`ManagerEntry` se usa cuando una capability administrativa necesita shell, routing, autorización y lifecycle Web sin inventar Source/Projection.

CURRENT observado:

```text
Users
→ ManagerEntry

Tool Catalog de Command Center
→ ManagerEntry

Profiles
→ ManagerModule

Alarm Configuration de Command Center
→ ManagerModule
```

No crear `ManagerModule` por simetría cuando el dominio no posee Source/Projection.

## Tres niveles que deben mantenerse separados

```text
CAPABILITY
    ↓
MANAGER COMPOSITION
    ↓
PRODUCT COMPOSITION
    ↓
HOST APPLICATION
```

Una aplicación standalone puede existir para desarrollo/qualification sin convertirse en el contrato que consume el producto final.

El consumidor integrado debe reutilizar la misma composition, no ejecutar otro host ni duplicar su implementación.

## ADA Configuration Manager CURRENT

`ada-configuration-manager` contiene hoy:

```text
composition/dependencies/wiring/workflows
+
application/local_runtime/__main__
```

Por tanto cumple dos roles físicos:

1. composition reusable del Manager ADA;
2. host standalone para desarrollo/qualification.

ADA Generic no ejecuta ese host. Importa `build_configuration_manager_surface(...)` y monta el `ManagerSurface` dentro de su propia `WebApplicationDefinition`.

El dual-role del package es **CURRENT** y puede considerarse deuda de naming/boundary, pero no autoriza un split incidental.

## Command Center Configuration Manager CURRENT

`ada-command-center-configuration-manager` es un host temporal que reutiliza:

```text
compose_alarm_configuration_manager(...)
create_tool_catalog_manager_entry(...)
```

No duplica Alarm Configuration ni Tool Catalog.

La composition que hoy produce su `ManagerSurfaceDefinition` debe ser el punto de integración administrativa de Command Center; el futuro host integrado no debe reconstruir Alarm/Tool Catalog por su cuenta.

## Compositions reusable — estado auditado

```text
users-manager
CURRENT / VERIFIED / consumed by ADA

profiles-manager
CURRENT / VERIFIED / consumed by ADA

navigation-manager
BLOCKED / reusable extraction incomplete / not consumed by ADA

Command Center alarm configuration manager composition
CURRENT / VERIFIED

Command Center tool catalog manager composition
CURRENT / VERIFIED
```

### Users

Users permanece `ManagerEntry`.

`UsersAdministrationService` es su lifecycle administrativo.

### Profiles

Profiles posee Source/Projection y usa `ManagerModule`.

Su composition registra servicios mediante su `WebModule`.

### Navigation

Navigation es la divergencia transversal que debe resolverse antes de que un segundo producto la consuma.

Finding VERIFIED en a7546574:

- ADA no depende de `atlanticus-web-composition-navigation-manager`;
- ADA mantiene su wiring/workflows de Navigation dentro de `ada-configuration-manager`;
- la composition reusable llama `ManagerAuthorizationPolicy.can_access(...)`, método que no existe en el contrato CURRENT, cuyo método es `can_view(...)`;
- la composition reusable registra servicios inmediatamente sobre un `ServiceRegistry` recibido por parámetro, distinto del patrón CURRENT de Profiles/Alarm;
- su workflow no es idéntico al workflow ADA: agrega validadores, validación de definición y controles adicionales de source/workspace/concurrencia;
- su `source_key` default es `navigation-configuration`, mientras ADA usa `navigation`;
- sus labels de Source/Projection están fijados en la composition reusable.

No corregir estos puntos por inferencia ni migrar ADA mecánicamente.

Primero se decide cuál comportamiento representa el contrato reusable correcto; luego se reemplaza limpiamente la solución anterior.

## Regla de composición entre capabilities

Una product composition puede traducir contratos neutrales entre capabilities independientes.

Ejemplo permitido:

```text
ProfileCatalog
    ↓ product composition
NavigationProfileOption
```

Esto no es un shim legacy.

Está prohibido mantener simultáneamente contratos viejos/nuevos mediante aliases o adapters de compatibilidad.

## Próximo frente

```text
MANAGER-COMPOSITION-CONVERGENCE-AND-DUAL-PRODUCT-INTEGRATION
PLANNED / NEXT
```

El frente debe terminar con:

```text
one Manager package/version authority
one reusable Manager composition contract
ADA Generic integrated
ADA Command Center integrated
tooling/distribution of both aligned
```

No abrir Resource Preparation, Live, Analytics, nueva UX ni migración Python/Trixie durante este frente.
