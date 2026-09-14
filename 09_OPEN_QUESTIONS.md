# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Estos puntos no bloquean el cierre del bootstrap. Se resuelven durante ejecución cuando corresponda.

## User Activity

1. partition key final;
2. deterministic id format;
3. exact session persistence mechanism across reload;
4. verify TTL 86400 physically.

## Resources

Cerrados para Users/resource preflight:

```text
WEB-STORAGE-TOPOLOGY              CLOSED / VERIFIED
USERS-STORAGE-TOPOLOGY            CLOSED / VERIFIED
STORAGE-PREFLIGHT-COSMOS-BRIDGE   CLOSED / VERIFIED
COSMOS-USERS-RUNTIME-ADAPTER      CLOSED / VERIFIED
USERS-RUNTIME-PROJECTION-BOUNDARY CLOSED / VERIFIED
```

`users.runtime` queda confirmado como:
- provider Cosmos;
- physical name `users-runtime`;
- partition key `/id`;
- TTL `None`;
- connection binding provisto por composición.

Continúan OPEN:

5. complete resource/container inventory after tracing Operaciones Integradas;
6. Storage provisioning parity;
7. cloud permissions for application resource creation;
8. integrar el bridge ya implementado dentro del lifecycle Web cuando se congelen `ApplicationResourcePlan`, required/optional, named connection resolution y readiness;
9. validar provisioning/validation real de `users.runtime` en los entornos que correspondan.

El cierre de `users.runtime` no congela la inventory global de recursos.

## Source / Blob

Cerrados por `SOURCE-1A.1` / `SOURCE-1A.2`:
- release identity;
- release granularity;
- functional manifest;
- SourceStore API;
- concurrency/current;
- Blob provider parity/recovery.

13. retention/cleanup policy permanece abierta fuera de `SourceStore`.

## Backend generation

14. freeze ProcessDefinition/generator current implementation after audit;
15. distribution metadata expected by external DevOps;
16. finish Python 3.14.7/Trixie migration.

## Frontend generation

17. reconcile/generate current ADA Generic artifact tooling;
18. final Web distribution contract.

## Manager

19. remove local full-access bypass;
20. implement Login/Bootstrap Console;
21. freeze Component External Links schema;
22. warmup/cache contract for links.

## KPI

23. implement/test common `REPROCESS_CURRENT`;
24. verify full Historian rebuild semantics on partial/missing materialization.

## Alarm / Command Center

25. finish B.2 runtime/materialization gap;
26. freeze Management payload/actions for ADA Web;
27. run History/Data Sufficiency qualification;
28. only then decide additional events/evidence;
29. analytics remains deferred until GREEN.

## Alarm preservation

30. detailed extraction of some early A-D qualification genealogy remains preservation backlog.
   Original sources remain retained; this does not block product work but must not be discarded.

## Scripts / Docs

31. component-level gates still missing in some areas;
32. master repo integrity gate;
33. env.detail enrichment;
34. final READMEs.

## Loaders

35. implement approved ADA loader;
36. Manager loader;
37. Command Center loader when its Web design closes.

## Navigation / Manager Source migration

Cerrados:
- Navigation canonical Source backend contracts;
- Navigation ProjectionStore Local/Cosmos;
- Navigation runtime consumer sobre ProjectionStore canónico;
- `MANAGER-ROOT-CANONICAL-CUTOVER` para la acción productiva Project.

Checkpoint Manager:

```text
moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
```

38. **CLOSED / VERIFIED / CURRENT** — el root productivo de Project ya usa `ProjectionTarget = SourceKey + SourceReleaseRef`, selecciona current server-side y transporta el target exacto sin releer current durante `project(target)`.
39. `PLANNED` — migrar el flujo administrativo Navigation al contrato canónico aplicable. Auditar primero el composition root real; no asumir una clase `NavigationManagerWorkflowAdapter` porque ese nombre no existe en el checkpoint auditado.
40. `BLOCKED` — eliminar Source/Projection legacy de Navigation sólo después de validar que todos sus consumidores productivos hayan migrado.
41. `PLANNED` — implementar persistencia browser de Manager WORKSPACE con IndexedDB + `dcc.Store(memory)`.
42. `PLANNED` — retirar SharePoint/Power Automate del pipeline Source únicamente cuando las rutas consumidoras correspondientes hayan migrado.

El item 38 anterior, que trataba el root Manager como pendiente, queda `SUPERSEDED AS OPEN ITEM` por el cierre verificado. Esto no cierra publicación/verificación/history administrativa basada todavía en revisiones textuales.

## Users / Profiles / Access

Dirección ya congelada:

```text
Profiles MUST NOT require Access.
Access MAY consume/extend Profiles.
```

Cerrado dentro de esta frontera:
- storage topology de `users.runtime`;
- un único recurso durable para Pending + Managed;
- partition `/id`;
- TTL `None`;
- connection binding por composición;
- bridge Cosmos entre topology y Connectivity;
- `CosmosUsersRuntimeStore` para `UsersRuntimeStore` + `PendingUsersReader`;
- semántica create-only de `observe()`;
- ownership del writer snapshot-level de Managed Users;
- transición Pending→Resolved por mismo id/partition;
- retiro durable como Resolved deshabilitado + `managed_state=retired`;
- re-add como `managed_state=present`;
- CAS/ETag sin blind upsert para actualización Managed;
- resolver de Access rechaza Managed deshabilitado antes de requerir perfil histórico;
- Source canónico de Users sobre `SourceStore` con `ConcurrencyToken`, `basis_release`, History y lectura de release exacta;
- Projection canónica de Users desde `SourceReleaseRef` exacta;
- `ProjectionRecord[UsersConfigurationCatalog]`;
- Cosmos ProjectionStore con provenance Source exact-release;
- idempotencia same-target y conflicto CAS explícito;
- root Project de Manager transportando target exacto;
- capability Profiles extraída físicamente de Users;
- dependencia one-way Users→Profiles;
- Profiles sin dependencia de Users ni ADA Access;
- eliminación del namespace Python productivo `atlanticus.web.users.profiles` sin shim.

Cierres durante ejecución:

43. `USERS-RUNTIME-PROJECTION-BOUNDARY`: `CLOSED / VERIFIED / CURRENT` en `moragaga/atlanticus@4758d993296bfe2a629a9aa3b8e4b486cf7b2305`.
44. transición Pending→Resolved, retiro/re-add, concurrencia con `observe()`, convergencia/replay y provenance legacy de runtime quedaron implementados y validados en el mismo checkpoint.

Además:

```text
USERS-CANONICAL-SOURCE-1       CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2   CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER CLOSED / VERIFIED / CURRENT
PROFILES-DOMAIN-EXTRACTION     CLOSED / VERIFIED / CURRENT
```

Checkpoints:

```text
USERS-CANONICAL-SOURCE-1
moragaga/atlanticus@f996905c353de26c42bc4907e32a1f2f0c161648

USERS-CANONICAL-PROJECTION-2
moragaga/atlanticus@139ee93a118e51f66c3d585f00235f212a2475c1

MANAGER-ROOT-CANONICAL-CUTOVER
moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06

PROFILES-DOMAIN-EXTRACTION
moragaga/atlanticus@b34581958e8d59f2cd14e47f56c4309ee76027fb
```

La extracción física está cerrada, pero la frontera semántica completa Users / Profiles / ADA Access sigue `IN PROGRESS`.

45. **CLOSED / VERIFIED FOR PHYSICAL OWNERSHIP** — se auditó lo suficiente la implementación y los consumidores directos para extraer `ProfileDefinition`, `ProfileCatalog`, constantes, normalizadores y su error de dominio a `web/capabilities/profiles/core`. Esto no equivale a auditar toda la semántica futura ni todos los bindings cross-capability.
46. **OPEN / UNVERIFIED** — extraer y reconciliar `manager_dispatched/Atlanticus_ADA_Usuarios_Perfiles_Acceso_Arquitectura_2026-09-10.docx`. Su existencia está verificada, pero su contenido no fue auditado en este hito.
47. **OPEN / NEXT** — congelar `PROFILES-BASELINE-SEMANTICS` antes de nuevas migraciones dependientes.
48. **CLOSED / SUPERSEDED AS OPEN ITEM** — `USERS-CANONICAL-PROJECTION-2` ya conecta Source exacta con Projection canónica. La formulación anterior que incluía también provenance exact-release de `users.runtime` queda refinada: esa parte sigue separada.
49. `PLANNED` — migrar el flujo administrativo Users después de cerrar la separación de contratos y auditar el composition root real. No asumir una clase `UsersManagerWorkflowAdapter` sin evidencia.
50. `BLOCKED` — eliminar contratos/adapters Source legacy de Users únicamente después de validar que no quedan consumidores productivos legacy.
51. `PLANNED / SUPERSEDED AS NEXT` — migrar el runtime Managed desde provenance legacy (`projection_source_revision`) a provenance exact-release. La precondición Manager root ya está satisfecha, pero el orden recomendado cambió para cerrar primero la frontera semántica Profiles/Users. No introducir shim `SourceReleaseId <-> str`.
52. `OPEN` — congelar y declarar, si corresponde, el resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`; el provider actual recibe `container_name`, pero `USERS-CANONICAL-PROJECTION-2` no fija physical name, TTL ni connection binding.
53. `OPEN` — validar el composition root productivo del canonical Users Projection store cuando corresponda al incremento de integración.
54. `CURRENT` — mantener separados canonical Projection y runtime legacy hasta que un cutover explícito reemplace limpiamente el provenance legacy.
55. **CLOSED / VERIFIED / CURRENT** — `PROFILES-DOMAIN-EXTRACTION`: capability `atlanticus-web-profiles==0.1.0`, Users→Profiles one-way, namespace viejo eliminado sin shim, suite Web 512 passed / 7 skipped y Ruff GREEN.
56. **PLANNED / NEXT** — `PROFILES-BASELINE-SEMANTICS`: resolver y congelar `root`, `guest`, `local`, John/Jane, `administrator` y la representación runtime de Guest sin tocar aún Source/Projection ni Access.
57. **PLANNED** — `USERS-CONTRACT-SEPARATION`: separar ownership de catálogos/contratos Users y Profiles sin romper los invariantes Source/Projection ya cerrados silenciosamente.
58. **PLANNED** — `USERS-PROFILES-ADMIN-COMPOSITION`: permitir una experiencia administrativa conjunta sin recombinar ownership de dominio.
59. **PLANNED** — `USERS-RUNTIME-CANONICAL-CUTOVER`: materializar runtime desde contratos ya separados y revisar entonces el provenance exact-release.

### OPEN semántico para el siguiente foco

Debe resolverse explícitamente, no por inferencia:
- si `guest` permanece representado por un `ProfileDefinition` sintético/runtime o por otro contrato base;
- ubicación y forma exacta del bootstrap principal `root`;
- claim/identity key inmutable que identifica al bootstrap principal;
- representación de `root` en el effective principal sin convertirlo en Profile normal;
- alcance mínimo de autorización bootstrap y eventual revocación/disable;
- ownership exacto de John/Jane y sus colores estáticos;
- destino de `LOCAL_PROFILE_KEY` y todos sus consumidores;
- retiro de `administrator` como system profile reservado de Atlanticus;
- regla para impedir que un Managed User apunte a un Profile eliminado/no proyectado;
- eventual separación de Source/Projection de Profiles y coordinación de publicaciones;
- destino de `PROFILE_CATALOG_SERVICE_KEY = 'atlanticus.web.users.profiles'`, que sigue siendo una service key vigente y no un import Python;
- estrategia explícita para superseder el payload combinado `ProjectionRecord[UsersConfigurationCatalog]` si la separación contractual posterior lo requiere.
