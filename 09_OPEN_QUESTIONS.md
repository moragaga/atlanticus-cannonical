# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos aquí no reabren contratos ya CLOSED.

## Users / Manager exact lifecycle

### CLOSED

Ya no están OPEN:

- productive exact-source registration Users en ADA;
- Users lifecycle legacy coexistence;
- Users exact Source read;
- Users exact publication;
- Users exact Projection status;
- Users exact Projection execution;
- Users exact History list/read;
- History preview Users;
- History release identity transport;
- load History como local work;
- destino de `UsersManagerWorkflowAdapter` en el host ADA.

Estado:

```text
USERS-EXACT-MANAGER-LIFECYCLE
CLOSED / VERIFIED / CURRENT
```

Contrato cerrado:

```text
workflow_service               None
draft_validation_service       exact/canonical
exact_source_reader_service    exact
exact_source_history_service   exact
exact_source_workflow_service  exact
exact_projection_service       exact
```

## OPEN — ADA legacy Projection contract alignment

Este es el único foco recomendado para el siguiente chat.

Affected:

1. `NavigationManagerWorkflowAdapter`
2. `ToolConfigurationManagerWorkflowAdapter`
3. `KpiConfigurationManagerWorkflowAdapter`
4. `KpiDefinitionManagerWorkflowAdapter`

VERIFIED gap:

- Manager `ConfigurationLifecycleWorkflow` vigente requiere `get_current_projection_target()`;
- `project(...)` recibe `ProjectionTarget`;
- `ProjectionExecutionResult` requiere `target`;
- adapters ADA legacy todavía exponen/esperan revisions textuales;
- full ADA suite actual: `56 passed, 4 failed`.

OPEN:

5. Definir el mínimo cutover limpio para esos adapters.
6. Determinar si cada dominio ya puede seleccionar `ProjectionTarget` exacto desde su workflow actual.
7. Separar cualquier dominio que requiera contrato adicional en vez de inventar target/provenance.
8. Reejecutar full ADA suite tras la alineación.
9. No tocar Users para resolver estos fallos.

## OPEN — Users residual fuera de Manager lifecycle

10. `USERS-RUNTIME-CANONICAL-CUTOVER`.
11. `USERS-RUNTIME-EXACT-RELEASE-PROVENANCE`.
12. `USERS-ADMIN-CANONICAL-MIGRATION` umbrella para consumidores legacy restantes fuera del Manager Users.
13. Destino final de `UsersAdministrationService` legacy.
14. Destino final de `UsersConfigurationBundle`.
15. Destino de otros consumers de `UsersConfigurationCatalog`.
16. Legacy deletion Users — BLOCKED hasta demostrar ausencia de consumers.
17. Resource topology/provisioning físico de canonical Users Projection store.
18. Composition root productivo concreto del canonical Users Projection store si corresponde.
19. UX explícita de `replacement_profile_key` al borrar Profile referenciado.
20. Qualification visual browser productiva del UI/History exacto si no existe evidencia separada.

## OPEN / UNVERIFIED — runtime assembly

21. Constructor físico externo que inyecta `ConfigurationManagerDependencies.users_profiles_administration`.
22. Constructor físico externo que inyecta `ConfigurationManagerDependencies.users_exact_projection`.
23. Docker E2E del host completo.

La ausencia de evidencia aquí no reabre la composición de aplicación ya verificada.

## Root bootstrap physical configuration

Contrato lógico CLOSED.

Permanece OPEN / UNVERIFIED:

24. fuente física de `BootstrapRootPolicy`;
25. almacenamiento/configuración de `issuer`;
26. almacenamiento/configuración de `subject_id`;
27. mapping exacto claims Entra;
28. lifecycle operativo disable/revoke;
29. integración Key Vault/env/config si corresponde.

No inventar secretos ni claims.

## Local / John / Jane

Boundary CLOSED:

- no son Profiles funcionales.

Permanece OPEN:

30. owner exacto contrato runtime Local;
31. representación runtime John/Jane;
32. colores/visuals estáticos definitivos;
33. interacción Local con `EffectiveUser`.

## Navigation / Manager Source migration

Cerrado:

- Navigation canonical Source;
- Navigation ProjectionStore Local/Cosmos;
- Navigation runtime canonical consumer;
- Manager root Project exact-target cutover;
- Manager generic exact-source boundaries.

OPEN:

34. ADA legacy Projection contract alignment del adapter Navigation;
35. migración administrativa Navigation más amplia;
36. Navigation legacy deletion — BLOCKED;
37. Manager WORKSPACE IndexedDB;
38. retiro SharePoint/Power Automate sólo cuando consumers migren.

## Resources

Cerrado:

- `users.runtime`;
- Storage Topology;
- Cosmos preflight bridge;
- Users Cosmos runtime adapter.

OPEN:

39. inventory completa de recursos;
40. Storage provisioning parity;
41. permisos cloud creación/validación;
42. lifecycle Web resource readiness;
43. provisioning/validation real `users.runtime` por entorno;
44. resource physical contract canonical Users Projection store.

## Source / Blob

Cerrado:

- release identity;
- immutable releases;
- SourceStore;
- concurrency/current;
- Local/Blob parity;
- exact-release Projection handoff;
- Users canonical multi-resource release;
- Manager exact Source publication/read/history boundaries;
- Users exact Manager lifecycle.

OPEN:

45. retention/cleanup policy fuera de `SourceStore`.

## User Activity

OPEN:

46. partition key final;
47. deterministic id format final;
48. persistence across reload;
49. TTL 86400 físico.

## Backend / Frontend generation

OPEN:

50. ProcessDefinition/generator audit/freeze;
51. external DevOps distribution metadata;
52. Python 3.14.7/Trixie global migration;
53. ADA Generic artifact tooling;
54. final Web distribution contract.

## Manager / Login

OPEN:

55. eliminar local full-access bypass donde corresponda;
56. Login/Bootstrap Console;
57. Component External Links schema;
58. warmup/cache contract.

## KPI

OPEN:

59. ADA legacy Projection alignment de KPI/KPI Definitions;
60. `REPROCESS_CURRENT`;
61. full Historian rebuild semantics.

## Alarm / Command Center

OPEN según documentos especializados:

62. B.2 runtime/materialization gap;
63. Management payload/actions ADA Web;
64. History/Data Sufficiency qualification;
65. analytics sólo después de GREEN.

## Scripts / Docs

OPEN:

66. component-level gates faltantes;
67. master repo integrity gate;
68. env.detail enrichment;
69. final READMEs.

## Loaders

OPEN:

70. ADA loader;
71. Manager loader;
72. Command Center loader cuando cierre Web design.

## Siguiente foco

```text
ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT
PLANNED / NEXT
```

No mezclarlo con runtime provenance, Python migration, Root physical config, legacy deletion global ni Users exact lifecycle.
