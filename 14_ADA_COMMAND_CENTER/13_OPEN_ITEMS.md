# ADA Command Center — Open Items

Estado: **OPEN / REFINED AFTER ALARM CONFIGURATION MILESTONE**

Los contracts globales Users/Profiles/ADA Access ya cerrados no se reabren desde Command Center.

Alarm Configuration contract, Source/Release, base Projection y Manager integration están
**CLOSED / VERIFIED / CURRENT**.

## Web application

1. definir/montar la aplicación Web propia de Command Center y su package/entrypoint;
2. shell/header final;
3. navegación/páginas iniciales Dashboard + Historia/Explorer + Configuración;
4. APIs/read models Web para Live/History/Analytics;
5. montar el `ManagerModule` CURRENT de Alarm Configuration dentro del shell final;
6. integrar Profiles/Navigation/permissions en la superficie final cuando corresponda.

La ubicación física de Alarm Configuration ya no es OPEN:

```text
scopes/ada-command-center/web/alarms/configuration
```

## Alarm Configuration refinements

7. binding productivo de `SourceStore`/provider Blob en la composición Command Center;
8. Message Catalog UI final;
9. editor visual de Rules;
10. editor visual genérico de parameters `str | float | bool`.

No crear otro schema durable para estos editores; deben operar sobre `AlarmConfiguration` CURRENT.

La metadata/schema específica por evaluator no es responsabilidad de Alarm Configuration.

## Command Center Tool Catalog — NEXT

11. congelar el contrato puro de `CommandCenterToolCatalog`/snapshot consumible por B.2;
12. definir la forma mínima de cada entry sin duplicar ADA Tool authoring;
13. definir provenance hacia la Tool projection externa y conexión nombrada;
14. definir representación contractual de AVAILABLE/STALE/MISSING sin confundirla con una referencia Alarm UNRESOLVED;
15. definir identity/revision del catálogo;
16. definir binding durable Blob/current/LKG;
17. declarar inputs Tool sobre conexiones Cosmos nombradas;
18. cadence/trigger de reconciliación;
19. startup/readiness/LKG del reconciliador;
20. wiring de credenciales/permisos read-only para Cosmos externos;
21. verificar el punto CURRENT de generación/enforcement de unicidad global de `tool_key`.

Frontera congelada para este siguiente hito:

```text
ADA Tool Configuration = authoring / owner
Command Center Tool Catalog = consolidator / reconciler / read-only derived state
```

No está abierto crear una segunda Tool Configuration authoring ni una segunda Tool Projection Cosmos
de Command Center por simetría.

## B.2 — PLANNED AFTER TOOL CATALOG CONTRACT

La implementación física B.2 fue verificada como ausente en el checkpoint CURRENT.

22. definir `ResolvedAlarmConfiguration` y su identity/provenance;
23. separar Runtime readiness de Delivery/reference readiness;
24. definir findings para evaluator/Tool/Component/Subcomponent/routing/visual target no resueltos;
25. materializar Runtime/Delivery desde una misma resolución;
26. reconciliar `alarm_configuration_revision`/`tool_registry_revision` históricos del runtime con provenance CURRENT;
27. decidir si/how evaluator resolution requiere identity/provenance adicional; `AlarmEvaluatorRegistry` CURRENT no expone revisión;
28. definir Live Projection schema después de cerrar la salida Delivery/resolution que necesita.

No iniciar estos puntos antes de cerrar el contrato Tool Catalog productor.

## History / Analytics

29. unidad del read model;
30. History sola vs History + Aggregates;
31. storage/indexing/partitioning;
32. retention;
33. calendar/turno;
34. duración de priority dispositions;
35. normalización/comparabilidad de Evidence;
36. insight rules;
37. límites de causalidad.

## Data update

38. cadence Live;
39. cadence/cache Analytics;
40. separar de auto-refresh de sesión.

## Golden Path

41. seleccionar Rule/evaluator/Tool real para la vertical integrada;
42. demostrar preconfiguración con Tool inicialmente no resuelta y resolución posterior sin republicar Alarm Source.

## Web platform bootstrap

43. integrar ApplicationResourcePlan;
44. definir containers propios de Command Center/Alarm backend;
45. integrar bootstrap surface/readiness;
46. definir projection order sólo donde existan dependencias reales;
47. definir si User Activity entra en Golden Path o incremento posterior;
48. aplicar mismo TTL/history contract si Activity se habilita.

## Cross-cutting conflict

El package Command Center CURRENT requiere Python `3.14.2`, mientras el baseline del Project declara
Python `3.14.7`.

Estado:

```text
CONFLICT / OPEN / OUTSIDE THIS MILESTONE
```

No corregirlo silenciosamente dentro de Tool Catalog.
