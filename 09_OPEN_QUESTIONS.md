# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos listados aquí no reabren contratos ya CLOSED.

## Users / Profiles / Identity

### Cerrado por PROFILES-BASELINE-SEMANTICS + UCS-1

Ya no están OPEN:
- si Guest debe ser `ProfileDefinition`;
- si Root pertenece a Profiles;
- bootstrap Root contract lógico;
- si Administrator es system profile especial de Profiles core;
- si `ProfileCatalog()` debe fabricar defaults;
- destino de `PROFILE_CATALOG_SERVICE_KEY`;
- si Users WebModule debe registrar ProfileCatalog;
- separación ownership durable Users vs Profiles;
- destino canónico de `administrator_*`;
- destino canónico de `guest_*`;
- payload canónico que reemplaza `ProjectionRecord[UsersConfigurationCatalog]`;
- regla canónica ante Managed User con Profile inexistente;
- necesidad de Source/Projection independiente de Profiles para este baseline.

Estado:

```text
PROFILES-BASELINE-SEMANTICS     CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION       CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT  CLOSED / VERIFIED / CURRENT
```

### OPEN

1. `USERS-PROFILES-ADMIN-COMPOSITION` — migrar authoring/admin a contratos separados sin recombinar ownership.
2. Definir payload/draft administrativo exacto para editar Users + Profiles antes de publicar la única exact Source release.
3. Definir UX de Profile delete/reassign cuando existen Managed Users referenciándolo; el contrato canónico ya rechaza orphan references.
4. Definir cómo migra `UsersAdministrationService` desde `UsersConfigurationCatalog`.
5. Definir destino de `UsersConfigurationBundle` y contracts legacy durante la migración administrativa.
6. Definir destino de `FileUsersProjectionProfileCatalog` dentro del cutover de consumidores.
7. `USERS-RUNTIME-CANONICAL-CUTOVER` — materializar runtime desde `UsersProfilesConfiguration`.
8. `USERS-RUNTIME-EXACT-RELEASE-PROVENANCE` — reemplazar provenance legacy sólo en cutover explícito.
9. `USERS-ADMIN-CANONICAL-MIGRATION` — completar migración administrativa tras la composición.
10. Legacy deletion Users — BLOCKED hasta demostrar ausencia de consumidores legacy.
11. Resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`.
12. Composition root productivo del canonical Users Projection store.

No está OPEN por defecto crear Source/Projection independiente de Profiles. UCS-1 resolvió el ownership actual con dos resources en una sola exact release. Sólo reabrir esa decisión si aparece un requisito de lifecycle/release independiente.

## Root bootstrap physical configuration

Contrato lógico CLOSED.

Permanece OPEN / UNVERIFIED:
13. fuente física de `BootstrapRootPolicy`;
14. dónde se almacena/configura `issuer`;
15. dónde se almacena/configura `subject_id`;
16. mapping exacto de claims Entra hacia `AuthenticatedIdentity.issuer/subject_id`;
17. lifecycle operativo para disable/revoke;
18. integración con Key Vault/env/config si corresponde.

No inventar nombres de secretos ni claims sin evidencia.

## Local / John / Jane

Boundary CLOSED:
- no son Profiles funcionales.

Permanece OPEN:
19. owner exacto del contrato runtime Local;
20. representación runtime John/Jane;
21. colores/visuals estáticos definitivos si deben congelarse contractualmente;
22. interacción de Local con `EffectiveUser`, dado que Resolved normal requiere Profile.

## Historical Users / Profiles / Access decision

23. `manager_dispatched/Atlanticus_ADA_Usuarios_Perfiles_Acceso_Arquitectura_2026-09-10.docx`.

Existencia: VERIFIED.

Contenido: UNVERIFIED en este cierre.

No hay conflicto concreto adjudicado con ese DOCX porque su contenido no fue auditado.

Debe revisarse antes de afirmar compatibilidad histórica completa de la frontera Users/Profiles/ADA Access.

## Navigation / Manager Source migration

Cerrado:
- Navigation canonical Source;
- Navigation ProjectionStore Local/Cosmos;
- Navigation runtime canonical consumer;
- Manager root Project exact-target cutover.

OPEN:
24. migración administrativa Navigation;
25. Navigation legacy deletion — BLOCKED hasta validar consumidores;
26. Manager WORKSPACE IndexedDB;
27. retiro de SharePoint/Power Automate sólo cuando consumidores correspondientes hayan migrado.

## Resources

Cerrado:
- `users.runtime`;
- Storage Topology;
- Cosmos preflight bridge;
- Users Cosmos runtime adapter.

OPEN:
28. inventory completa de recursos;
29. Storage provisioning parity;
30. permisos cloud para creación/validación;
31. lifecycle Web de resource readiness;
32. provisioning/validation real de `users.runtime` por entorno;
33. resource physical contract del canonical Users Projection store.

## Source / Blob

Cerrado:
- release identity;
- immutable releases;
- functional manifest;
- SourceStore;
- concurrency/current;
- Local/Blob parity;
- exact-release Projection handoff;
- Users canonical multi-resource exact-release.

OPEN:
34. retention/cleanup policy fuera de `SourceStore`.

## User Activity

OPEN:
35. partition key final;
36. deterministic id format;
37. persistence mechanism across reload;
38. TTL 86400 físico.

## Backend / Frontend generation

OPEN:
39. ProcessDefinition/generator audit/freeze;
40. external DevOps distribution metadata;
41. Python 3.14.7/Trixie global migration;
42. ADA Generic artifact tooling;
43. final Web distribution contract.

## Manager / Login

OPEN:
44. eliminar local full-access bypass donde aún corresponda;
45. Login/Bootstrap Console;
46. Component External Links schema;
47. warmup/cache contract.

El contrato Root bootstrap lógico ya implementado no resuelve por sí solo estos puntos.

## KPI

OPEN:
48. `REPROCESS_CURRENT`;
49. full Historian rebuild semantics.

## Alarm / Command Center

OPEN según sus documentos especializados:
50. B.2 runtime/materialization gap;
51. Management payload/actions para ADA Web;
52. History/Data Sufficiency qualification;
53. analytics sólo después de GREEN.

## Scripts / Docs

OPEN:
54. component-level gates faltantes;
55. master repo integrity gate;
56. env.detail enrichment;
57. final READMEs.

## Loaders

OPEN:
58. ADA loader;
59. Manager loader;
60. Command Center loader cuando cierre su Web design.

## Siguiente foco

Único foco recomendado:

```text
USERS-PROFILES-ADMIN-COMPOSITION  PLANNED / NEXT
```

No mezclarlo con:
- runtime canonical cutover;
- runtime exact-release provenance;
- Root physical configuration;
- Navigation;
- Python 3.14.7;
- Manager IndexedDB;
- ADA Access;
- legacy cleanup general.
