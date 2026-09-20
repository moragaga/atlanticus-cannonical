# Manager — Navigation and Home

Estado: **CURRENT / MODULES + ENTRIES**

## `/manager`

`/manager` es una Home real.

No representa el primer item administrativo y no redirige implícitamente a
Users/Profiles/Navigation/Tools.

## Registry

`ManagerModuleRegistry` es la fuente única de items administrativos visibles y rutas.

Mantiene:

```text
modules
entries
items = modules + entries
```

Home, sidebar y routing derivan del mismo registry para evitar divergencia.

## Visibilidad

La secuencia CURRENT es:

```text
ManagerModuleRegistry
        ↓
visible_items(principal, policy)
        ↓
render Home / sidebar
```

Las vistas específicas siguen disponibles:

```text
visible_modules(...)
visible_entries(...)
```

No renderizar un item antes de comprobar autorización.

## Home

Responsabilidad:

- descubrir items administrativos visibles;
- mostrar estado resumido cuando exista;
- abrir la capability correspondiente.

No debe absorber workflow de edición/publicación.

### Estado

`ManagerModule` puede mostrar estado Source/Projection porque ese lifecycle existe.

`ManagerEntry` no debe recibir un badge Projection ficticio sólo para mantener simetría
visual.

## Navegación Manager

Manager conserva navegación administrativa propia:

- botón/trigger lateral;
- Home;
- grupos;
- items administrativos;
- retorno explícito a Manager Home;
- sidebar administrativa.

No fusionar este mecanismo con la navegación operacional principal de ADA.

## Grupos CURRENT en ADA Configuration Manager

La composition CURRENT registra:

```text
administration
configuration
```

Users pertenece a `administration`.

Profiles, Navigation, Tools, KPI Configuration y KPI Definition pertenecen al flujo de
configuration según la composition CURRENT.

Los grupos responden a una necesidad real de organización y no crean nuevas fronteras de
dominio.

## Paginación

La Home mantiene page size 6.

La paginación de presentación no debe provocar lecturas repetidas de Source cuando el
snapshot ya está hidratado.

La paginación específica de Users es responsabilidad de su Web surface y usa el contrato
genérico de paginación Web ya existente.

## Principio

La navegación Manager organiza capacidades administrativas.

La navegación ADA organiza herramientas/superficies operacionales.

Son responsabilidades distintas aunque convivan en el mismo ecosistema.
