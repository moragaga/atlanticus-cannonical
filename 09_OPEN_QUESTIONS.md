# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Los puntos listados aquí no reabren contratos ya CLOSED.

## Users / Profiles / Identity

### Cerrado por PROFILES-BASELINE-SEMANTICS

Ya no están OPEN:
- si Guest debe ser `ProfileDefinition`;
- si Root pertenece a Profiles;
- bootstrap Root contract lógico;
- si Administrator es system profile especial de Profiles core;
- si `ProfileCatalog()` debe fabricar defaults;
- destino de `PROFILE_CATALOG_SERVICE_KEY`;
- si Users WebModule debe registrar ProfileCatalog;
- si Local/Guest deben mostrarse como Profiles en Users Configuration.

Estado:

```text
PROFILES-BASELINE-SEMANTICS  CLOSED / VERIFIED / CURRENT
```

### OPEN

1. `USERS-CONTRACT-SEPARATION` — separar ownership durable/contractual Users vs Profiles preservando Source/Projection vigente.
2. Definir el destino de `administrator_*` y `guest_*` dentro del aggregate durable actual durante esa separación.
3. Definir estrategia explícita para superseder `ProjectionRecord[UsersConfigurationCatalog]` sólo si la separación contractual lo exige.
4. Definir regla final cuando un Managed User referencia un Profile eliminado/no proyectado.
5. Decidir si Profiles necesita Source/Projection propia o si otro contrato satisface el requisito; no crearla por inferencia.
6. `USERS-PROFILES-ADMIN-COMPOSITION` — experiencia administrativa conjunta sin recombinar ownership.
7. `USERS-RUNTIME-CANONICAL-CUTOVER` — materializar runtime desde contratos separados.
8. `USERS-RUNTIME-EXACT-RELEASE-PROVENANCE` — reemplazar provenance legacy sólo en cutover explícito.
9. Migración administrativa Users al contrato canónico.
10. Legacy deletion Users — BLOCKED hasta demostrar ausencia de consumidores legacy.
11. Resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`.
12. Composition root productivo del canonical Users Projection store.

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
- exact-release Projection handoff.

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
USERS-CONTRACT-SEPARATION  PLANNED / NEXT
```

No mezclarlo con:
- Root physical configuration;
- runtime exact-release provenance;
- Admin migration;
- Navigation;
- Python 3.14.7;
- Manager IndexedDB;
- ADA Access;
- legacy cleanup general.
