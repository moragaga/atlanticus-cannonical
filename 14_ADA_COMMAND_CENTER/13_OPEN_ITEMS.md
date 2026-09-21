# ADA Command Center — Open Items

Estado: **OPEN / REFINED**

Los contracts globales Users/Profiles/ADA Access ya cerrados no se reabren desde Command Center.

## Web

1. ubicación física;
2. shell/header final;
3. páginas iniciales;
4. APIs/read models Web;
5. integración exacta de la superficie genérica Manager dentro del shell de Command Center.

## Alarm Configuration

6. binding físico Source/Release para Rules + Messages;
7. Message Catalog UI final;
8. editor genérico de parameters `str | float | bool`;
9. integración de administración Profiles/Navigation en la superficie final;
10. ADA Access Configuration UI;
11. ADA Access Projection persistence;
12. ADA Access runtime composition.

La metadata/schema específica por evaluator deja de ser open item: Alarm Configuration no la posee.

La relación global `user -> profile_key` ya pertenece a Users CURRENT y no es un open item de Command Center.

## Tool Catalog

13. contrato físico del snapshot/revision en Blob;
14. declaración de inputs Tool sobre conexiones Cosmos nombradas;
15. cadence/trigger de reconciliación;
16. política exacta AVAILABLE/STALE/MISSING y diagnóstico;
17. startup/readiness/LKG del catálogo;
18. wiring de credenciales/permisos read-only para Cosmos externos;
19. verificar el punto de generación/enforcement de unicidad global de `tool_key`.

No está abierto crear una segunda Tool Projection Cosmos en Command Center: esa duplicación queda fuera de la dirección actual.

## B.2

20. verificar implementación física actual;
21. definir `ResolvedAlarmConfiguration` y su identity/provenance;
22. separar Runtime readiness de Delivery/reference readiness;
23. definir findings para evaluator/Tool/Component/Subcomponent/routing/visual target no resueltos;
24. materializar Runtime/Delivery desde una misma resolución;
25. Live Projection schema;
26. reemplazar/reconciliar los revision strings históricos del runtime con provenance de resolución CURRENT.

## History / Analytics

27. unidad del read model;
28. History sola vs History + Aggregates;
29. storage/indexing/partitioning;
30. retention;
31. calendar/turno;
32. duración de priority dispositions;
33. normalización/comparabilidad de Evidence;
34. insight rules;
35. límites de causalidad.

## Data update

36. cadence Live;
37. cadence/cache Analytics;
38. separar de auto-refresh de sesión.

## Golden Path

39. seleccionar Rule/evaluator/Tool real para la vertical integrada;
40. demostrar preconfiguración con Tool inicialmente no resuelta y resolución posterior sin republicar Alarm Source.

## Web platform bootstrap

41. integrar ApplicationResourcePlan;
42. definir containers propios de Command Center/Alarm backend;
43. integrar bootstrap surface/readiness;
44. definir projection order sólo donde existan dependencias reales;
45. definir si User Activity entra en Golden Path o incremento posterior;
46. aplicar mismo TTL/history contract si Activity se habilita.
