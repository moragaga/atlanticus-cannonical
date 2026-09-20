# Manager — Application Boundary

Estado: **CURRENT**

## Decisión

Manager tiene una frontera de aplicación propia.

No es una variante del shell operacional de ADA ni debe reutilizar el header operacional de
ADA como si fueran la misma superficie.

## Implementación verificada

La aplicación ADA Configuration Manager crea un `ManagerSurface` y lo monta como una
aplicación independiente.

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

La Home del Manager construye su propio header administrativo.

## Items administrativos CURRENT

El shell Manager admite dos tipos de item registrados:

```text
ManagerModule
ManagerEntry
```

`ManagerModule` representa capabilities con Source/Projection.

`ManagerEntry` representa capabilities administrativas que necesitan el mismo shell,
routing, authorization y WebModule lifecycle, pero que no poseen Source/Projection.

La existencia de `ManagerEntry` no convierte toda frontera lógica en un servicio ni en un
nuevo lifecycle.

## Regla canónica

- ADA operacional → header/shell ADA.
- Manager → header/shell Manager.
- Pueden compartir primitives, tokens, branding o capacidades transversales.
- No comparten ownership de navegación ni header.
- La presentación específica permanece en la capability que la posee.

Esto es una frontera de aplicación, no una excepción CSS.

## Reusabilidad

Manager sigue siendo capability genérica Atlanticus y no depende directamente de conceptos
ADA.

ADA Configuration Manager compone capacidades genéricas y ADA-specific sobre esa capability.

CURRENT incluye:

```text
ManagerModule:
- Profiles
- Navigation
- Tools
- KPI Configuration
- KPI Definition

ManagerEntry:
- Users
```

Profiles y Users permanecen capabilities genéricas Atlanticus.

Navigation, Tools, KPI Configuration y KPI Definition son consumers/configurations del
producto ADA según sus contratos actuales.

## Bootstrap boundary

Existe una superficie anterior a Manager para primera instalación/readiness.

No comparte ownership con el shell Manager.

```text
Bootstrap/System Surface
        ↓
Manager readiness
        ↓
Manager Shell
```

El Manager continúa teniendo header/shell propios una vez habilitado.
