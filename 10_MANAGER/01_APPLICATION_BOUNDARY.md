# Manager — Application Boundary

Estado: **CURRENT**

## Decisión

Manager tiene una frontera de aplicación propia.

No es una variante del shell operacional de ADA ni debe reutilizar el header operacional de
ADA como si fueran la misma superficie.

## Implementación CURRENT

ADA Configuration Manager crea un `ManagerSurface` y lo monta como aplicación independiente.

La capability genérica `atlanticus.web.manager` posee:

- surface;
- registry;
- authorization;
- lifecycle;
- coordinator Source/Projection;
- Home;
- layout;
- callbacks;
- assets.

## Items administrativos CURRENT

```text
ManagerModule
ManagerEntry
```

`ManagerModule` representa capabilities con Source/Projection.

`ManagerEntry` representa capabilities administrativas que necesitan el mismo shell, routing,
authorization y WebModule lifecycle sin inventar Source/Projection.

## Regla canónica

- ADA operacional → header/shell ADA.
- Manager → header/shell Manager.
- Pueden compartir primitives, tokens, branding o comportamiento transversal real.
- No comparten ownership de navegación ni header.
- La presentación específica permanece en la capability que la posee.

## Reusabilidad

Manager sigue siendo capability generic Atlanticus.

ADA Configuration Manager compone capabilities generic y ADA-specific sobre Manager.

CURRENT:

```text
ManagerModule:
- Profiles / título ADA: Perfiles
- Accesos
- Navigation
- Tools
- KPI Configuration
- KPI Definition

ManagerEntry:
- Users
```

Access es application-specific ADA.

Profiles, Users y Navigation son generic Atlanticus.

La composition ADA puede localizar títulos visibles sin cambiar el default generic de la
capability.

## Navigation configuration boundary

La UI/configuration de Navigation pertenece a Navigation.

Manager provee shell/workflow/composition, no ownership de la presentación específica de
Navigation.

Navigation Configuration no depende de Profiles; la adaptación de profile options ocurre en
la application/composition que conoce ambas capabilities.

## Access configuration boundary

La UI/configuration de ADA Access pertenece a Access.

Manager provee shell/workflow/composition, no ownership de la presentación específica de
Access.

Access puede consumir `ProfileCatalog` porque esa dependencia pertenece a su contrato de
dominio. Esto no autoriza mover Profiles dentro de Manager ni crear una dependencia inversa
desde Profiles hacia ADA Access.

La UI de Access puede usar el catálogo de sistema como fallback de presentación; la validation
de draft sigue exigiendo Profiles Projection activa.

## UI qualification boundary

```text
MANAGER-UI-CONSISTENCY-REVIEW
IN PROGRESS
```

Slices ya cerrados:

```text
Navigation
Accesos
```

Siguiente slice:

```text
Perfiles
```

Corregir shared Manager CSS sólo cuando el problema sea realmente transversal.

No mover CSS capability-local a Manager por simetría.

La fase responsive/media-query transversal se ejecuta después de coherencia visual de las
páginas.

## Bootstrap boundary

```text
Bootstrap/System Surface
        ↓
Manager readiness
        ↓
Manager Shell
```

Bootstrap no comparte ownership con el shell Manager.
