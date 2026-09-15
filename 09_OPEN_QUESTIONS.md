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
- ubicación/dirección de dependencias del adapter exact-source Users↔Manager;
- si la composition exact-source debe rebasar el draft;
- migración de active callbacks/layout/store Users al payload canónico;
- política ante browser draft schema 1/incompatible: descartar y crear clean desde current Source;
- Profile create/edit sobre `ProfilesConfiguration` dentro de `UsersProfilesConfiguration`;
- Administrator editor canónico;
- Managed add desde Pending y Managed edit con identidad inmutable;
- save/load local schema 2;
- dirty revision local del editor;
- eliminación de `UsersConfigurationCatalog` del active editor path;
- import file legacy: decode explícito + split a canonical payload sin migrar browser draft.

Estados:

```text
ADMIN-COMPOSITION-BACKEND                     CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY                 CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-DRAFT-BASELINE-SEMANTICS CLOSED / VERIFIED / CURRENT
USERS-MANAGER-EXACT-SOURCE-COMPOSITION        CLOSED / VERIFIED / CURRENT
ADMIN-UI-DRAFT-CUTOVER                        CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-COMPOSITION              IN PROGRESS
```

### OPEN — productive Manager exact-source cutover

1. `USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER`.
2. Reemplazar el service registration productivo `UsersManagerWorkflowAdapter` para publication exact-source sin adaptar el payload canónico a `UsersConfigurationCatalog`.
3. Migrar publication action productiva a `ManagerProjectionCoordinator.publish_draft_exact(...)`.
4. Definir qué contrato lifecycle legacy debe coexistir con `ExactSourcePublicationWorkflow` para validate/project/history durante el cutover.
5. Verificar si el mismo service object debe satisfacer ambos contratos o si el host debe registrar superficies separadas.
6. Confirmar cómo el Manager productivo consume el draft schema 2 y exact `SourceSnapshot`; no asumir compatibilidad con `ManagerDraft` schema 1.
7. Decidir presentation UI/audit de la nueva release sin reintroducir `source_revision` como identidad exacta.
8. Verificar el constructor/wiring productivo real de `ConfigurationManagerDependencies.users_profiles_administration`; no fue localizado dentro de `atlanticus`.
9. `USERS-ADMIN-CANONICAL-MIGRATION`: completar migración una vez UI + productive Manager cutover estén cerrados.
10. Destino final de `UsersAdministrationService` legacy.
11. Destino final de `UsersConfigurationBundle` y contracts legacy.
12. Destino de `FileUsersProjectionProfileCatalog` dentro del cutover de consumidores.
13. Legacy deletion Users — BLOCKED hasta demostrar ausencia de consumidores legacy.
14. Resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`.
15. Composition root productivo del canonical Users Projection store.

### OPEN — Users admin UI residual

16. UX explícita para elegir `replacement_profile_key` al borrar un Profile referenciado. El backend contract ya está CLOSED; la UI actual simplemente evita ese delete.
17. History preview Users sigue legacy; decidir su cutover/retirada dentro de la migración administrativa correspondiente.
18. Validar visualmente el nuevo UI canónico en browser productivo si aún no existe qualification visual registrada.

### OPEN — ADA host packaging / Manager contract alignment

19. Resolver/adjudicar el drift del lock de `ada-configuration-manager`:

```text
atlanticus-web-manager             lock 0.3.14 / source 0.3.15
atlanticus-web-users-configuration lock 0.1.6  / source 0.1.9
```

20. Determinar el mínimo cambio necesario para que el host pueda calificarse con el Manager vigente sin arrastrar silenciosamente KPI/Navigation/Tools al incremento Users.
21. Cinco failures observados con overlay Manager 0.3.15 corresponden a adapters ADA legacy con contrato anterior de Projection; decidir frontera antes de modificar.
22. Reejecutar full ADA suite sólo cuando el entorno/contratos estén coherentes; último full overlay observado no fue GREEN y no se volvió a correr después del fix de mirror.

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

Python qualification del checkpoint `d23bff...` en 3.14.7 permanece BLOCKED porque el intérprete no estaba disponible localmente; el cierre se calificó con 3.14.2.

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
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER
PLANNED / NEXT CANDIDATE
```

El primer paso del siguiente chat es debate/auditoría del host actual. Si cerrar Users obliga a migrar adapters no relacionados de KPI/Navigation/Tools, no ampliar alcance silenciosamente: marcar el bloqueo y separar frontera.

No mezclarlo con:
- runtime canonical cutover;
- runtime exact-release provenance;
- Root physical configuration;
- Navigation migration;
- Python 3.14.7 global migration;
- Manager IndexedDB general;
- ADA Access;
- legacy cleanup general.
