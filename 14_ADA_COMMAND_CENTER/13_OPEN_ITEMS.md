# ADA Command Center — Open Items

Estado: **OPEN / REFINED AFTER TOOL CATALOG + AUTHORING REFERENCES V1**

Los contracts globales Users/Profiles/ADA Access ya cerrados no se reabren desde Command Center.

Cerrado en este hito:

```text
COMMAND-CENTER-TOOL-CATALOG-V1
COMMAND-CENTER-ALARM-TOOL-REFERENCES-V1
```

## Siguiente foco único — Alarm Configuration structured authoring

```text
ALARM-CONFIGURATION-STRUCTURED-AUTHORING-V1
PLANNED / NEXT
```

Debe integrar en la UI el read model CURRENT:

```text
AlarmToolReferenceCatalog
→ Tool
→ Component
→ Subcomponent
```

Invariantes:

- no cambiar `AlarmConfiguration` durable;
- persistir keys, no display names;
- mantener authoring no restrictivo cuando no exista catálogo o una referencia no esté sugerida;
- no introducir B.2 dentro de callbacks/layout;
- no duplicar reglas de `ToolStructure`.

No ampliar automáticamente este incremento a Message editor completo, parameters editor completo,
B.2 o application shell.

## Alarm Configuration refinements posteriores

- binding productivo de `SourceStore`/provider Blob en la composición Command Center;
- Message Catalog UI final;
- editor visual completo de Rules;
- editor visual genérico de parameters `str | float | bool`.

No crear otro schema durable para estos editores.

## Tool Catalog operational composition

El contrato y Blob store V1 están CURRENT. Permanece OPEN sólo la composición operacional real:

- declarar los inputs Tool reales de Command Center;
- construir/injectar sus `ProjectionStore[ToolConfiguration]` con conexiones nombradas;
- configurar Storage credentials/container/blob del catálogo;
- decidir dónde/cuándo se ejecuta `ToolCatalogConsolidator.refresh()`;
- definir cadence/retry sólo si la operación real lo necesita;
- integrar startup/readiness/observability del refresh;
- verificar permisos read-only sobre Cosmos externos.

No reabrir por defecto:

- AVAILABLE/STALE/MISSING;
- history de catálogo;
- LKG separado;
- segundo Cosmos de Command Center.

Esas extensiones sólo deben volver a discusión por necesidad demostrada.

## B.2 — PLANNED

La implementación física B.2 continúa ausente.

Permanece OPEN:

- definir `ResolvedAlarmConfiguration` y su identity/provenance;
- combinar Alarm Source/Projection revision + Tool Catalog revision;
- separar Runtime readiness de Delivery/reference readiness;
- findings para evaluator/Tool/Component/Subcomponent/routing/visual target no resueltos;
- materializar Runtime/Delivery desde una misma resolución;
- reconciliar `alarm_configuration_revision`/`tool_registry_revision` históricos del runtime;
- decidir provenance de evaluator si realmente se necesita;
- Live Projection schema después de cerrar resolution/delivery.

No mezclar B.2 con el siguiente incremento de UI authoring.

## Web application

Permanece OPEN:

- aplicación Web propia de Command Center y entrypoint;
- shell/header final;
- navegación Dashboard + Historia/Explorer + Configuración;
- montaje final del `ManagerModule` dentro del shell;
- integración final de Profiles/Navigation/permissions.

## History / Analytics

Permanece OPEN:

- unidad del read model;
- History sola vs History + Aggregates;
- storage/indexing/partitioning;
- retention;
- calendar/turno;
- duración de priority dispositions;
- normalización/comparabilidad de Evidence;
- insight rules;
- límites de causalidad.

## Data update

Permanece OPEN:

- cadence Live;
- cadence/cache Analytics;
- separación respecto de auto-refresh de sesión.

## Golden Path

Permanece OPEN:

- seleccionar Rule/evaluator/Tool real para la vertical integrada;
- demostrar re-resolution de la misma Alarm Source revision cuando una Tool aparece posteriormente.

## Web platform bootstrap

Permanece OPEN:

- `ApplicationResourcePlan`;
- containers/configuración productiva propios de Command Center/Alarm backend;
- bootstrap surface/readiness;
- projection order sólo donde existan dependencias reales;
- decisión sobre User Activity.

## Cross-cutting conflict

Packages CURRENT de este frente requieren Python `3.14.2`, mientras el baseline del Project declara
Python `3.14.7`.

Estado:

```text
CONFLICT / OPEN / OUTSIDE THIS MILESTONE
```

No corregirlo silenciosamente dentro de structured authoring ni B.2.
