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
- identidad local del draft canónico;
- source basis del draft canónico;
- política backend de Profile delete/reassign;
- si Administrator puede eliminarse;
- si Managed puede crearse arbitrariamente sin Pending;
- mutabilidad de identidad de Managed;
- forma de transportar exact-source publication en Manager sin reinterpretar strings legacy.

Estados:

```text
PROFILES-BASELINE-SEMANTICS       CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION         CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT    CLOSED / VERIFIED / CURRENT
ADMIN-COMPOSITION-BACKEND         CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY     CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-COMPOSITION  IN PROGRESS
```

### OPEN — siguiente frontera administrativa

1. `ADMIN-UI-DRAFT-CUTOVER`: migrar callbacks/layout/store productivo al nuevo contrato.
2. Definir comportamiento UI exacto cuando exista en browser un draft legacy incompatible con el nuevo `document_type`; no crear adapter de payload legacy al nuevo draft.
3. Migrar Profile editor para operar sobre `ProfilesConfiguration` dentro de `UsersProfilesConfiguration`.
4. Migrar Administrator editor a `update_administrator_colors(...)`.
5. Migrar Profile delete para solicitar replacement explícito cuando existan referencias.
6. Migrar User editor para alta desde Pending y actualización con identidad inmutable.
7. Migrar save/load browser draft a `UsersProfilesAdminDraft`.
8. Eliminar del camino UI activo la reconstrucción de `UsersConfigurationCatalog`, sin borrar aún contratos legacy que tengan otros consumidores.
9. Auditar preview/import/history UI porque actualmente pueden asumir payload legacy.

### OPEN — después del UI cutover

10. `USERS-MANAGER-EXACT-SOURCE-WIRING`: workflow productivo Users todavía no implementa el protocolo exact-source.
11. Decidir ubicación concreta del adapter/composition que une Manager con `UsersProfilesAdministrationService`, evitando dependencia circular o ownership incorrecto.
12. Migrar publication action para usar `ManagerProjectionCoordinator.publish_draft_exact(...)`.
13. Definir cómo se presenta en UI/audit la nueva release publicada sin volver a introducir `source_revision` como identidad exacta.
14. `USERS-ADMIN-CANONICAL-MIGRATION`: completar migración administrativa una vez UI + Manager wiring estén cerrados.
15. Destino final de `UsersAdministrationService` legacy.
16. Destino final de `UsersConfigurationBundle` y contracts legacy.
17. Destino de `FileUsersProjectionProfileCatalog` dentro del cutover de consumidores.
18. Legacy deletion Users — BLOCKED hasta demostrar ausencia de consumidores legacy.
19. Resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`.
20. Composition root productivo del canonical Users Projection store.

### Runtime

21. `USERS-RUNTIME-CANONICAL-CUTOVER`.
22. `USERS-RUNTIME-EXACT-RELEASE-PROVENANCE`.

No está OPEN por defecto crear Source/Projection independiente de Profiles.

## Root bootstrap physical configuration

Contrato lógico CLOSED.

Permanece OPEN / UNVERIFIED:
23. fuente física de `BootstrapRootPolicy`;
24. dónde se almacena/configura `issuer`;
25. dónde se almacena/configura `subject_id`;
26. mapping exacto claims Entra;
27. lifecycle operativo disable/revoke;
28. integración Key Vault/env/config si corresponde.

No inventar secretos ni claims.

## Local / John / Jane

Boundary CLOSED:
- no son Profiles funcionales.

Permanece OPEN:
29. owner exacto contrato runtime Local;
30. representación runtime John/Jane;
31. colores/visuals estáticos definitivos;
32. interacción Local con `EffectiveUser`.

## Historical Users / Profiles / Access decision

33. `manager_dispatched/Atlanticus_ADA_Usuarios_Perfiles_Acceso_Arquitectura_2026-09-10.docx`.

Existencia: VERIFIED.

Contenido: UNVERIFIED en este cierre.

## Navigation / Manager Source migration

Cerrado:
- Navigation canonical Source;
- Navigation ProjectionStore Local/Cosmos;
- Navigation runtime canonical consumer;
- Manager root Project exact-target cutover;
- Manager generic exact-source opt-in boundary.

OPEN:
34. migración administrativa Navigation;
35. Navigation legacy deletion — BLOCKED;
36. Manager WORKSPACE IndexedDB;
37. retiro SharePoint/Power Automate sólo cuando consumidores migren.

## Resources

Cerrado:
- `users.runtime`;
- Storage Topology;
- Cosmos preflight bridge;
- Users Cosmos runtime adapter.

OPEN:
38. inventory completa de recursos;
39. Storage provisioning parity;
40. permisos cloud creación/validación;
41. lifecycle Web resource readiness;
42. provisioning/validation real `users.runtime` por entorno;
43. resource physical contract canonical Users Projection store.

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
- generic Manager exact-source publication boundary.

OPEN:
44. retention/cleanup policy fuera de `SourceStore`.

## User Activity

OPEN:
45. partition key final;
46. deterministic id format;
47. persistence across reload;
48. TTL 86400 físico.

## Backend / Frontend generation

OPEN:
49. ProcessDefinition/generator audit/freeze;
50. external DevOps distribution metadata;
51. Python 3.14.7/Trixie global migration;
52. ADA Generic artifact tooling;
53. final Web distribution contract.

## Manager / Login

OPEN:
54. eliminar local full-access bypass donde corresponda;
55. Login/Bootstrap Console;
56. Component External Links schema;
57. warmup/cache contract.

## KPI

OPEN:
58. `REPROCESS_CURRENT`;
59. full Historian rebuild semantics.

## Alarm / Command Center

OPEN según documentos especializados:
60. B.2 runtime/materialization gap;
61. Management payload/actions ADA Web;
62. History/Data Sufficiency qualification;
63. analytics sólo después de GREEN.

## Scripts / Docs

OPEN:
64. component-level gates faltantes;
65. master repo integrity gate;
66. env.detail enrichment;
67. final READMEs.

## Loaders

OPEN:
68. ADA loader;
69. Manager loader;
70. Command Center loader cuando cierre Web design.

## Siguiente foco

Único foco recomendado:

```text
ADMIN-UI-DRAFT-CUTOVER  PLANNED / NEXT
```

No mezclarlo con:
- Users↔Manager exact-source wiring;
- runtime canonical cutover;
- runtime exact-release provenance;
- Root physical configuration;
- Navigation;
- Python 3.14.7;
- Manager IndexedDB general;
- ADA Access;
- legacy cleanup general.
