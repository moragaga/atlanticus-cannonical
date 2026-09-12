# Manager — Application Boundary

Estado: **CURRENT**

## Decisión

Manager tiene una frontera de aplicación propia.

No es una variante del shell operacional de ADA ni debe reutilizar el header operacional de ADA como si fueran la misma superficie.

## Implementación verificada

La aplicación ADA Configuration Manager crea un `ManagerSurface` y lo monta como una aplicación independiente.

La capability genérica `atlanticus.web.manager` posee:

- surface;
- registry;
- authorization;
- lifecycle;
- coordinator;
- projection;
- Home;
- layout;
- callbacks;
- assets.

La Home del Manager construye su propio header administrativo.

## Regla canónica

- ADA operacional → header/shell ADA.
- Manager → header/shell Manager.
- Pueden compartir primitives, tokens, branding o capacidades transversales.
- No comparten ownership de navegación ni header.

Esto es una frontera de aplicación, no una excepción CSS.

## Reusabilidad

Manager debe seguir siendo capability genérica Atlanticus y no depender directamente de conceptos ADA.

ADA Configuration Manager compone módulos ADA sobre esa capability genérica.

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
