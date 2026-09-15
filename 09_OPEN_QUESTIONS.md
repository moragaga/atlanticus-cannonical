# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos listados aquí no reabren contratos ya CLOSED.

## Users / Profiles / Identity

### Cerrado

Ya no están OPEN:
- si Guest debe ser `ProfileDefinition`;
- si Root pertenece a Profiles;
- bootstrap Root contract lógico;
- si Administrator es system profile especial de Profiles core;
- si `ProfileCatalog()` debe fabricar defaults;
- destino de `PROFILE_CATALOG_SERVICE_KEY`;
- separación ownership durable Users vs Profiles;
- destino canónico de `administrator_*`;
- destino canónico de `guest_*`;
- payload canónico Projection;
- regla canónica ante User con Profile inexistente;
- necesidad de Source/Projection independiente de Profiles para este baseline;
- payload backend de authoring Users+Profiles;
- source basis del draft canónico;
- política backend de Profile delete/reassign;
- si Administrator puede eliminarse;
- si Managed puede crearse arbitrariamente sin Pending;
- mutabilidad de identidad de Managed;
- contrato genérico Manager exact-source;
- semántica local clean/dirty/rebase del draft canónico;
- ubicación del adapter exact-source Users↔Manager;
- dirección de dependencias del adapter exact-source;
- si la composition exact-source debe rebasar el draft.

Estados:

```text
ADMIN-COMPOSITION-BACKEND                     CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY                 CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-DRAFT-BASELINE-SEMANTICS CLOSED / VERIFIED / CURRENT
USERS-MANAGER-EXACT-SOURCE-COMPOSITION        CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-COMPOSITION              IN PROGRESS
```

### OPEN — siguiente frontera administrativa

1. `ADMIN-UI-DRAFT-CUTOVER`: migrar callbacks/layout/store productivo al nuevo contrato.
2. Definir comportamiento UI exacto cuando exista en browser un draft legacy incompatible con schema 2; no crear adapter legacy→nuevo.
3. Migrar Profile editor para operar sobre `ProfilesConfiguration` dentro de `UsersProfilesConfiguration`.
4. Migrar Administrator editor a la operación canónica dedicada.
5. Migrar Profile delete para solicitar replacement explícito cuando existan referencias.
6. Migrar User editor para alta desde Pending y actualización con identidad inmutable.
7. Migrar save/load browser draft a `UsersProfilesAdminDraft` schema 2.
8. Usar `revision/base_payload_revision` para dirty/clean local.
9. Eliminar del camino UI activo la reconstrucción de `UsersConfigurationCatalog`, sin borrar aún contratos legacy con consumidores.
10. Auditar preview/import/history UI porque pueden asumir payload legacy.

### OPEN — productive Manager exact-source cutover

11. `USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER`.
12. Reemplazar el service registration productivo `UsersManagerWorkflowAdapter` sólo cuando el payload/UI canónico esté listo.
13. Migrar publication action productiva a `ManagerProjectionCoordinator.publish_draft_exact(...)`.
14. Decidir presentation UI/audit de la nueva release sin volver a introducir `source_revision` como identidad exacta.
15. Confirmar si el host productivo necesita además contratos legacy en el mismo objeto o si debe hacer cutover limpio.
16. `USERS-ADMIN-CANONICAL-MIGRATION`: completar migración una vez UI + productive Manager cutover estén cerrados.
17. Destino final de `UsersAdministrationService` legacy.
18. Destino final de `UsersConfigurationBundle` y contracts legacy.
19. Destino de `FileUsersProjectionProfileCatalog` dentro del cutover de consumidores.
20. Legacy deletion Users — BLOCKED hasta demostrar ausencia de consumidores legacy.
21. Resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`.
22. Composition root productivo del canonical Users Projection store.

### Runtime

23. `USERS-RUNTIME-CANONICAL-CUTOVER`.
24. `USERS-RUNTIME-EXACT-RELEASE-PROVENANCE`.

No está OPEN por defecto crear Source/Projection independiente de Profiles.

## Root bootstrap physical configuration

Contrato lógico CLOSED.

Permanece OPEN / UNVERIFIED:
25. fuente física de `BootstrapRootPolicy`;
26. dónde se almacena/configura `issuer`;
27. dónde se almacena/configura `subject_id`;
28. mapping exacto claims Entra;
29. lifecycle operativo disable/revoke;
30. integración Key Vault/env/config si corresponde.

No inventar secretos ni claims.

## Local / John / Jane

Boundary CLOSED:
- no son Profiles funcionales.

Permanece OPEN:
31. owner exacto contrato runtime Local;
32. representación runtime John/Jane;
33. colores/visuals estáticos definitivos;
34. interacción Local con `EffectiveUser`.

## Navigation / Manager Source migration

Cerrado:
- Navigation canonical Source;
- Navigation ProjectionStore Local/Cosmos;
- Navigation runtime canonical consumer;
- Manager root Project exact-target cutover;
- Manager generic exact-source opt-in boundary.

OPEN:
35. migración administrativa Navigation;
36. Navigation legacy deletion — BLOCKED;
37. Manager WORKSPACE IndexedDB;
38. retiro SharePoint/Power Automate sólo cuando consumidores migren.

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
- functional manifest;
- SourceStore;
- concurrency/current;
- Local/Blob parity;
- exact-release Projection handoff;
- Users canonical multi-resource release;
- generic Manager exact-source publication boundary;
- Users↔Manager exact-source composition adapter.

OPEN:
45. retention/cleanup policy fuera de `SourceStore`.

## User Activity

Ownership CURRENT:
- tracking funcional vive en `users/activity`;
- Identity aporta actor/auth context;
- Navigation no posee historial de actividad;
- `navigation-activity` sólo resuelve route keys desde Navigation.

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
59. `REPROCESS_CURRENT`;
60. full Historian rebuild semantics.

## Alarm / Command Center

OPEN según documentos especializados:
61. B.2 runtime/materialization gap;
62. Management payload/actions ADA Web;
63. History/Data Sufficiency qualification;
64. analytics sólo después de GREEN.

## Scripts / Docs

OPEN:
65. component-level gates faltantes;
66. master repo integrity gate;
67. env.detail enrichment;
68. final READMEs.

## Loaders

OPEN:
69. ADA loader;
70. Manager loader;
71. Command Center loader cuando cierre Web design.

## Siguiente foco

Único foco recomendado:

```text
ADMIN-UI-DRAFT-CUTOVER  PLANNED / NEXT
```

No mezclarlo con:
- productive exact-source service cutover;
- runtime canonical cutover;
- runtime exact-release provenance;
- Root physical configuration;
- Navigation migration;
- Python 3.14.7;
- Manager IndexedDB general;
- ADA Access;
- legacy cleanup general.
