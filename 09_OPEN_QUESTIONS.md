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
- Navigation runtime consumer sobre ProjectionStore canónico.

38. migrar el contrato productivo raíz de Manager desde `source_revision: str` a BASE/SOURCE/WORKSPACE/PROJECTION canónico;
39. migrar `NavigationManagerWorkflowAdapter` y el flujo administrativo Navigation al contrato Manager canónico;
40. después de validar todos los consumidores, eliminar Source/Projection legacy de Navigation y sus adapters históricos;
41. implementar persistencia browser de Manager WORKSPACE con IndexedDB + `dcc.Store(memory)`;
42. retirar SharePoint/Power Automate del pipeline Source sólo cuando las rutas consumidoras correspondientes hayan migrado.

## Users / Profiles / Access

Dirección ya decidida:

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
- sin dependencia de ADA Access.

Cerrados durante ejecución:

43. `USERS-RUNTIME-PROJECTION-BOUNDARY`: `CLOSED / VERIFIED / CURRENT` en `moragaga/atlanticus@4758d993296bfe2a629a9aa3b8e4b486cf7b2305`.
44. transición Pending→Resolved, retiro/re-add, concurrencia con `observe()`, convergencia/replay y provenance legacy de runtime quedaron implementados y validados en el mismo checkpoint.

Además:

```text
USERS-CANONICAL-SOURCE-1      CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2  CLOSED / VERIFIED / CURRENT
```

Checkpoints:

```text
USERS-CANONICAL-SOURCE-1
moragaga/atlanticus@f996905c353de26c42bc4907e32a1f2f0c161648

USERS-CANONICAL-PROJECTION-2
moragaga/atlanticus@139ee93a118e51f66c3d585f00235f212a2475c1
```

Continúa OPEN la frontera completa Users / Profiles / ADA Access y el cutover administrativo/runtime legacy:

45. auditar implementación actual restante de Users/Profile y consumidores, sin reabrir los contratos ya cerrados salvo conflicto autoritativo explícito;
46. extraer y reconciliar `Atlanticus_ADA_Usuarios_Perfiles_Acceso_Arquitectura_2026-09-10.docx`;
47. congelar la frontera restante Profiles / ADA Access y los bindings cross-capability antes de iniciar las migraciones dependientes;
48. **CLOSED / SUPERSEDED AS OPEN ITEM** — `USERS-CANONICAL-PROJECTION-2` ya conecta Source exacta con Projection canónica en `139ee93a118e51f66c3d585f00235f212a2475c1`. La formulación anterior que incluía también provenance exact-release de `users.runtime` queda refinada: esa parte no fue cerrada por este hito;
49. migrar `UsersManagerWorkflowAdapter` y el flujo administrativo Users sólo después del Manager root productive cutover de la pregunta 38;
50. eliminar contratos/adapters Source legacy de Users únicamente después de validar que no quedan consumidores productivos legacy;
51. migrar el runtime Managed desde provenance legacy (`projection_source_revision`) a provenance exact-release únicamente después del cutover raíz de Manager, sin shim `SourceReleaseId <-> str`;
52. congelar y declarar, si corresponde, el resource topology/provisioning físico de `CosmosUsersConfigurationProjectionStore`; el provider actual recibe `container_name`, pero `USERS-CANONICAL-PROJECTION-2` no fija physical name, TTL ni connection binding;
53. validar el composition root productivo del canonical Users Projection store cuando corresponda al incremento de integración;
54. mantener separados canonical Projection y runtime legacy hasta que el cutover raíz permita reemplazar limpiamente el camino anterior.
