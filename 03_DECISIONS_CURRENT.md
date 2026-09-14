# Atlanticus — Current Decisions

Estado: **CURRENT**

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / NOT YET IMPLEMENTED |
| `python:3.14.7-slim-trixie` | DECIDED / NOT YET IMPLEMENTED |
| `uv`, no pip normal | CURRENT |
| `backend/` representa backend jobs; no todo Python server-side | CURRENT |
| Server-side Python con responsabilidad Web pertenece a `web/` | CURRENT |
| Connectivity es dual-use y no adquiere ownership funcional | CURRENT |
| Web Storage Topology pertenece a `web/capabilities/storage/topology` | CURRENT / IMPLEMENTED + VALIDATED |
| `StorageResourceContract` es neutral: sin secretos, SDK clients ni I/O | FROZEN / IMPLEMENTED + VALIDATED |
| V1 sólo contempla overrides genéricos `connection_ref` y `physical_name`, sujetos a autorización de la capability | FROZEN / IMPLEMENTED + VALIDATED |
| owner/provider/topology no son overrides de composición en V1 | FROZEN / IMPLEMENTED + VALIDATED |
| Declaraciones idénticas del mismo `logical_id` deduplican; incompatibles fallan | FROZEN / IMPLEMENTED + VALIDATED |
| Override desconocido/prohibido y connection binding faltante fallan antes del provider | FROZEN / IMPLEMENTED + VALIDATED |
| Dos logical resources no pueden resolver al mismo `(provider, connection_ref, physical_name)` | FROZEN / IMPLEMENTED + VALIDATED |
| `CosmosContainerTopology` contiene partition key + TTL, no physical name | FROZEN / IMPLEMENTED + VALIDATED |
| `CosmosContainerSpec` permanece primitive de Connectivity; Web no depende del SDK/provider físico para declarar topology | CURRENT |
| `users.runtime` es el único recurso durable confirmado de Users | FROZEN / IMPLEMENTED + VALIDATED |
| `users.runtime` usa Cosmos, physical name `users-runtime`, partition `/id`, TTL `None` | FROZEN / IMPLEMENTED + VALIDATED |
| Users requiere `connection_ref` de composición y sólo permite ese override | FROZEN / IMPLEMENTED + VALIDATED |
| Pending y Managed comparten `users.runtime`; no se separan físicamente sin requisito independiente | FROZEN / IMPLEMENTED + VALIDATED |
| Datos durables de Users no deben expirar automáticamente por Cosmos TTL | FROZEN / IMPLEMENTED + VALIDATED |
| Bridge Cosmos de Storage vive en `web/capabilities/storage/cosmos` | CURRENT / IMPLEMENTED + VALIDATED |
| Bridge recibe `ResolvedStoragePlan` + provisioners Cosmos preconstruidos; no secretos/settings/client construction | FROZEN / IMPLEMENTED + VALIDATED |
| Bridge valida topology/specs/bindings antes del primer provider I/O | FROZEN / IMPLEMENTED + VALIDATED |
| Bridge no crea database; política local/cloud pertenece a resource preparation | FROZEN / IMPLEMENTED + VALIDATED |
| `CosmosUsersRuntimeStore` vive en package provider-specific separado de Users core | CURRENT / IMPLEMENTED + VALIDATED |
| Users Cosmos document usa `id == partition key == build_user_key(issuer, subject_id)` | FROZEN / IMPLEMENTED + VALIDATED |
| Users Cosmos distingue `record_type = pending | resolved` | FROZEN / IMPLEMENTED + VALIDATED |
| `observe()` de Users Cosmos es create-only + conflict reread y nunca upsert | FROZEN / IMPLEMENTED + VALIDATED |
| `list_pending()` de Users Cosmos es cross-partition y determinista | FROZEN / IMPLEMENTED + VALIDATED |
| Users Cosmos no provisiona database/container y recibe el physical container desde composición | FROZEN / IMPLEMENTED + VALIDATED |
| Managed writer hacia `users.runtime` no pertenece a `UsersRuntimeStore`; usa contrato snapshot-level `UsersRuntimeProjectionWriter` | FROZEN / IMPLEMENTED + VALIDATED |
| `UsersRuntimeMaterializingProjectionRepository` materializa runtime antes de avanzar el estado/catálogo de proyección legacy | FROZEN / IMPLEMENTED + VALIDATED |
| `CosmosUsersRuntimeProjectionWriter` vive en package provider-specific separado y no modifica el runtime adapter | CURRENT / IMPLEMENTED + VALIDATED |
| Writer Managed posee `resolved + is_local=false`; Local Resolved queda fuera de su ownership | FROZEN / IMPLEMENTED + VALIDATED |
| Pending→Resolved mantiene mismo `id`/partition; usuario configurado no observado puede crearse Resolved | FROZEN / IMPLEMENTED + VALIDATED |
| Usuario Managed removido no se borra: queda Resolved, disabled y `managed_state=retired` | FROZEN / IMPLEMENTED + VALIDATED |
| Re-add restaura `managed_state=present` y valores actuales de configuración | FROZEN / IMPLEMENTED + VALIDATED |
| Updates Managed usan ETag/CAS y nunca blind upsert | FROZEN / IMPLEMENTED + VALIDATED |
| Replay de snapshot Managed converge semánticamente; fallo runtime no avanza estado global legacy | FROZEN / IMPLEMENTED + VALIDATED |
| Managed deshabilitado se rechaza antes de requerir perfil histórico | FROZEN / IMPLEMENTED + VALIDATED |
| Manager posee shell/header administrativo propio | CURRENT |
| ADA Generic usa shell/header operacional ADA | CURRENT |
| Manager ≠ ADA operational shell | CURRENT |
| Source genérico pertenece a `web/capabilities/source` | CURRENT |
| Projection exact-release genérica pertenece a `web/capabilities/projection/core` | CURRENT / IMPLEMENTED + VALIDATED |
| Blob es Source durable productivo disponible para dominios migrados | CURRENT / IMPLEMENTED + VALIDATED |
| Local y Blob deben compartir semántica Source | FROZEN |
| Releases Source son publicaciones completas e inmutables | FROZEN |
| `release_id` identifica publicación y no equivale a `content_hash` | FROZEN |
| Dos releases pueden compartir `content_hash` | FROZEN |
| `manifest.json` es el único commit point de publicación | FROZEN |
| Draft no crea Source release | CURRENT |
| Restore histórico crea release nuevo | FROZEN |
| Restore nunca repunta current directamente a un release histórico | FROZEN |
| `previous_published_release` y `basis_release` son conceptos distintos | FROZEN |
| `ConcurrencyToken` es opaco para consumidores | FROZEN |
| El provider Source aplica la precondición autoritativa de concurrencia | FROZEN |
| No existe bypass `force=True` de concurrencia | FROZEN |
| History funcional sigue sólo la cadena de publicaciones alcanzable desde current | FROZEN |
| Un candidato que pierde CAS es orphan y no History | FROZEN |
| SourceStore publica cuando se le ordena; el no-op por mismo hash pertenece al consumidor | FROZEN |
| Projection identifica `source_release_id` concreto | FROZEN / IMPLEMENTED + VALIDATED |
| Projection target ejecutable = `SourceKey + SourceReleaseRef` | FROZEN / IMPLEMENTED + VALIDATED |
| `project(target)` resuelve la release exacta y no consulta Source current | FROZEN / IMPLEMENTED + VALIDATED |
| Projection alignment y attempt outcome son dimensiones separadas | FROZEN / IMPLEMENTED + VALIDATED |
| `CURRENT / OUTDATED` compara `SourceReleaseId`, no `content_hash` | FROZEN / IMPLEMENTED + VALIDATED |
| Projection failure no revierte Source | FROZEN |
| Retry de Projection conserva el mismo target sin republish de Source | FROZEN / IMPLEMENTED + VALIDATED |
| Cosmos nunca determina Source current | FROZEN |
| Navigation Source usa Source Core; no reimplementa history/release/CAS | CURRENT / IMPLEMENTED + VALIDATED |
| Navigation Projection Local/Cosmos implementan `ProjectionStore[NavigationConfigurationCatalog]` | CURRENT / IMPLEMENTED + VALIDATED |
| Navigation runtime consume `ProjectionStore[NavigationConfigurationCatalog] + SourceKey` | CURRENT / IMPLEMENTED + VALIDATED |
| Users Source usa Source Core; no reimplementa history/release/CAS | CURRENT / IMPLEMENTED + VALIDATED |
| Users Source resource canónico es `users/configuration.json.gz` con schema de dominio `1` | FROZEN / IMPLEMENTED + VALIDATED |
| `UsersSourceService.publish_catalog` usa `ConcurrencyToken` + `basis_release`, no `expected_source_revision: str` | FROZEN / IMPLEMENTED + VALIDATED |
| `UsersConfigurationBundle.revision` sigue siendo digest de contenido legacy y no equivale a `SourceReleaseId` | FROZEN |
| Users canonical Projection payload = `ProjectionRecord[UsersConfigurationCatalog]` | FROZEN / IMPLEMENTED + VALIDATED |
| Users canonical Projection usa `UsersSourceCodec` + validación de catálogo | FROZEN / IMPLEMENTED + VALIDATED |
| Users canonical Projection target = `SourceKey + SourceReleaseRef` | FROZEN / IMPLEMENTED + VALIDATED |
| Users canonical Projection provenance = `source_key + source_release_id + source_published_at_utc + projected_at_utc` | FROZEN / IMPLEMENTED + VALIDATED |
| Users canonical Projection no usa `UsersConfigurationBundle.revision` | FROZEN / IMPLEMENTED + VALIDATED |
| Users Cosmos canonical Projection first write es create-only; replace usa ETag/CAS; nunca blind upsert | FROZEN / IMPLEMENTED + VALIDATED |
| Retry same exact release + same payload es idempotente y conserva el active existente | FROZEN / IMPLEMENTED + VALIDATED |
| Same release ID con metadata o payload incompatible falla como invariante | FROZEN / IMPLEMENTED + VALIDATED |
| Conflicto concurrente same-target converge; different-target falla explícitamente | FROZEN / IMPLEMENTED + VALIDATED |
| Users Cosmos Projection no infiere ordering por release ID ni timestamps | FROZEN / IMPLEMENTED + VALIDATED |
| Users canonical Projection provider recibe `container_name` desde composición | FROZEN / IMPLEMENTED + VALIDATED |
| Manager root Project contract usa `ProjectionTarget = SourceKey + SourceReleaseRef` | FROZEN / IMPLEMENTED + VALIDATED |
| Manager workflow expone `get_current_projection_target()` y `project(target: ProjectionTarget)` | FROZEN / IMPLEMENTED + VALIDATED |
| `ProjectionExecutionResult.target` conserva la identidad Source exacta ejecutada | FROZEN / IMPLEMENTED + VALIDATED |
| `ManagerProjectionCoordinator.project(...)` transporta el target sin degradarlo a string ni releer current | FROZEN / IMPLEMENTED + VALIDATED |
| Project callback selecciona current server-side inmediatamente antes de ejecutar | FROZEN / IMPLEMENTED + VALIDATED |
| Browser state no es autoridad para la identidad ejecutable de Project | FROZEN / IMPLEMENTED + VALIDATED |
| Project signal expone `source_key + source_release_id + source_published_at_utc + projection_revision` | FROZEN / IMPLEMENTED + VALIDATED |
| Target histórico explícito en Manager Project no se reemplaza por current | FROZEN / IMPLEMENTED + VALIDATED |
| Manager root Project cutover no implica migración de publicación/verificación/history administrativa | FROZEN REFINEMENT |
| Users runtime exact-release provenance permanece separado del canonical Projection | CURRENT DIRECTION / PLANNED |
| No introducir shim `SourceReleaseId <-> str` para completar migraciones Manager/dominio | FROZEN |
| No crear un segundo Manager coordinator canónico paralelo | FROZEN |
| No asumir `UsersManagerWorkflowAdapter` ni `NavigationManagerWorkflowAdapter` como clases existentes sin evidencia | CURRENT |
| Manager browser WORKSPACE persistirá en IndexedDB | DECIDED / NOT YET IMPLEMENTED |
| Manager active workspace en Dash usa `dcc.Store(memory)` | DECIDED / NOT YET IMPLEMENTED |
| IndexedDB no es Source authority | FROZEN |
| Profiles debe funcionar sin Access | FROZEN DIRECTION |
| ADA Access puede consumir/extender Profiles; Profiles no depende de ADA Access | FROZEN DIRECTION |
| Profiles core pertenece a `web/capabilities/profiles/core` | CURRENT / IMPLEMENTED + VALIDATED |
| Profiles no depende de Users | FROZEN OWNERSHIP / IMPLEMENTED + VALIDATED |
| Users core depende one-way de Profiles | FROZEN OWNERSHIP / IMPLEMENTED + VALIDATED |
| Users Configuration declara Profiles como dependencia directa cuando consume contratos Profile | CURRENT / IMPLEMENTED + VALIDATED |
| `atlanticus.web.users.profiles` queda eliminado como namespace Python productivo | SUPERSEDED / IMPLEMENTED + VALIDATED |
| No existe shim/re-export de compatibilidad para `atlanticus.web.users.profiles` | FROZEN CUTOVER / IMPLEMENTED + VALIDATED |
| Errores propios de Profiles usan `ProfilesDefinitionError`, no `UsersDefinitionError` | CURRENT / IMPLEMENTED + VALIDATED |
| `PROFILE_CATALOG_SERVICE_KEY = 'atlanticus.web.users.profiles'` sigue siendo service key vigente; no es import namespace | CURRENT / NOT YET REASSESSED |
| Semántica actual `local + administrator + guest` de `ProfileCatalog` fue preservada durante la extracción | CURRENT IMPLEMENTATION / NOT FROZEN AS FINAL SEMANTICS |
| `root` fuera de Profiles y fuera de asignación Users normal | PROPOSED / AGREED DIRECTION / NOT YET IMPLEMENTED |
| `guest` como baseline Pending/unresolved fuera de Profiles proyectados | PROPOSED / AGREED DIRECTION / NOT YET IMPLEMENTED |
| John/Jane como identidades locales con colores estáticos | PROPOSED / AGREED DIRECTION / NOT YET IMPLEMENTED |
| perfiles funcionales (`administrator`, `operator`, `viewer`, custom) provenientes de Profiles proyectados | PROPOSED / AGREED DIRECTION / NOT YET IMPLEMENTED |
| Tool Configuration determina existencia estructural | FROZEN |
| Data determina estado | FROZEN |
| Component = Store + Collector contract + KPI destination | FROZEN |
| Subcomponent no crea Store/Collector propio | FROZEN |
| Data granularity != Alarm visual granularity | FROZEN |
| ADA Generic integra configuración/capacidades y habilita Tool específica | CURRENT DIRECTION |
| No construir capability `collectors` sin mapear producers/sources actuales | CURRENT |
| Tests sin validación CSS visual | CURRENT |
| No tests de existencia/no existencia de funciones internas | CURRENT |
| Procesos manuales/semi-automáticos son válidos | CURRENT |
| Alarm Definition B.1 | DESIGN FROZEN |
| Alarm B.2 Live != Management | DECISION RECORDED |
| Alarm B.2 I1 publication/runtime/delivery | DECISION RECORDED |
| Alarm B.2 I2 `LATEST SAVED = LATEST VALID` | DECISION RECORDED |
| R3.5 Alarm final qualification | CLOSED PASS/GREEN |

## Source / Projection / Resource Preparation checkpoint

```text
SOURCE-1A.1                       Core + Local                       CLOSED / VERIFIED
SOURCE-1A.2                       Blob                               CLOSED / VERIFIED
Projection                        Exact-release Core                 CLOSED / VERIFIED
NAV-SOURCE-PROJECTION-1           Source contracts                   CLOSED / VERIFIED
NAV-SOURCE-PROJECTION-2           Local/Cosmos Projection stores     CLOSED / VERIFIED
NAV-CONSUMER-MIGRATION-A          Runtime consumer                   CLOSED / VERIFIED
NAV-CONSUMER-MIGRATION-B          Administrative consumer            PLANNED
WEB-STORAGE-TOPOLOGY              Resource contracts                 CLOSED / VERIFIED
USERS-STORAGE-TOPOLOGY            users.runtime                      CLOSED / VERIFIED
STORAGE-PREFLIGHT-COSMOS-BRIDGE   Cosmos resource preflight          CLOSED / VERIFIED
COSMOS-USERS-RUNTIME-ADAPTER      Runtime store/reader               CLOSED / VERIFIED
USERS-RUNTIME-PROJECTION-BOUNDARY Managed runtime writer             CLOSED / VERIFIED
USERS-CANONICAL-SOURCE-1          Canonical Source backend           CLOSED / VERIFIED
USERS-CANONICAL-PROJECTION-2      Canonical exact-release Projection CLOSED / VERIFIED
MANAGER-ROOT-CANONICAL-CUTOVER    Root Project exact-target          CLOSED / VERIFIED
PROFILES-DOMAIN-EXTRACTION        Profiles core ownership            CLOSED / VERIFIED
PROFILES-BASELINE-SEMANTICS       Root/Guest/Local/Admin semantics   PLANNED / NEXT
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE Runtime provenance            PLANNED
```

Implementación actual relevante:

```text
web/capabilities/storage/topology
web/capabilities/storage/cosmos
web/capabilities/profiles/core
web/capabilities/users/core
web/capabilities/users/cosmos
web/capabilities/users/configuration
web/capabilities/users/projection-cosmos
web/capabilities/source/core
web/capabilities/source/local
web/capabilities/source/blob
web/capabilities/projection/core
web/capabilities/navigation/configuration
web/capabilities/manager
scopes/ada/web/application/ada-configuration-manager
```

Storage Topology:
- declara recursos físicos sin secretos ni clientes provider;
- resuelve bindings y conflictos antes del provider;
- expone `CosmosContainerTopology` sin acoplar las capabilities Web a `connectivity/cosmos`.

Cosmos Storage Bridge:
- traduce el plan neutral a `CosmosContainerSpec`;
- agrupa por `connection_ref` y usa provisioners preconstruidos;
- valida errores locales antes de I/O;
- no decide database creation ni Web readiness.

Users Storage Topology:
- declara un único `users.runtime`;
- usa `users-runtime`, `/id`, `TTL=None`;
- exige `connection_ref` de composición;
- prohíbe override del nombre físico;
- no separa Pending y Managed en containers distintos.

Users Cosmos Runtime Adapter:
- implementa `UsersRuntimeStore` y `PendingUsersReader`;
- point-read para resolve;
- create-only + conflict reread para observe;
- query cross-partition para Pending;
- no posee el writer administrativo de Managed Users.

Users Runtime Projection:
- `UsersRuntimeProjectionWriter` es contrato snapshot-level provider-neutral;
- `UsersRuntimeMaterializingProjectionRepository` materializa runtime antes del commit global legacy;
- `CosmosUsersRuntimeProjectionWriter` posee materialización Managed provider-specific;
- removal produce retired tombstone durable, no delete;
- promotion/update usa CAS/ETag y no blind upsert;
- `UsersRuntimeStore` y `PendingUsersReader` permanecen sin cambios;
- provenance todavía usa `projection_source_revision` legacy y permanece `PLANNED` para cutover posterior.

Users Canonical Source:
- `UsersSourceCodec` mapea `UsersConfigurationCatalog + published_by` a `SourceResource`;
- `UsersSourceService` usa `SourceStore` con release identity, History y CAS canónicos;
- mismo contenido puede republicarse como otra release;
- `UsersConfigurationBundle.revision` no se usa como identidad canónica de publicación.

Users Canonical Projection:
- `UsersProjectionBuilder` decodifica la release exacta y produce `UsersConfigurationCatalog`;
- `SourceProjectionService` ejecuta `ProjectionTarget(SourceKey + SourceReleaseRef)`;
- `CosmosUsersConfigurationProjectionStore` implementa `ProjectionStore[UsersConfigurationCatalog]`;
- provenance canónico usa release identity;
- create-only + ETag/CAS;
- same-target retry idempotente;
- different-target conflict explícito;
- no ordering inferido.

Profiles Domain Extraction:
- `atlanticus-web-profiles==0.1.0` es workspace package propio;
- `ProfileDefinition`, `ProfileCatalog`, constantes y normalizadores pertenecen a `atlanticus.web.profiles.models`;
- `ProfilesDefinitionError` pertenece a `atlanticus.web.profiles.errors`;
- Profiles no importa Users;
- Users importa Profiles one-way;
- Users Configuration declara Profiles directamente;
- old namespace eliminado sin shim;
- semántica del catálogo no fue rediseñada en este incremento.

Blob reutiliza `connectivity/storage` y conserva ETag como detalle técnico interno.
El prerequisito técnico `upload_if_match` quedó incorporado y validado en Storage; no hay otra carencia de Connectivity pendiente para Source Blob.

Projection Core:
- recibe targets exactos como `SourceKey + SourceReleaseRef`;
- persiste provenance con `source_release_id`;
- no revalida contra un `latest` mutable durante `project(target)`;
- permite retry del mismo target sin republish;
- separa alignment durable (`NEVER_PROJECTED / CURRENT / OUTDATED`) del outcome del intento (`SUCCESS / FAILED`).

Navigation:
- publica recursos de configuración mediante `SourceStore`;
- History proviene de Source;
- mismo contenido puede republicarse como una release distinta;
- implementa Projection stores concretos Local/Cosmos;
- runtime usa el `ProjectionStore` canónico;
- migración administrativa queda `PLANNED` y legacy deletion `BLOCKED` hasta validar consumidores.

Manager:
- `workspace.py` modela BASE/SOURCE/WORKSPACE/PROJECTION con `SourceSnapshot`, `ConcurrencyToken`, `basis_release` y `ProjectionTarget`;
- root Project quedó migrado a `ProjectionTarget` exacto en `5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06`;
- callback selecciona target server-side y `project(target)` no relee current;
- publication/verification/history textual permanece como contrato administrativo separado y no debe reinterpretarse como release identity.

No se consideran cerrados por los hitos anteriores ni por `PROFILES-DOMAIN-EXTRACTION`:
- provisioning/validation real de `users.runtime` dentro del lifecycle Web;
- resource topology/provisioning físico del canonical Users Projection store;
- provenance exact-release dentro de `users.runtime`;
- otros providers Projection concretos por dominio;
- orchestration multi-capability;
- derived resolutions;
- Navigation administrative consumer migration;
- Users administrative consumer migration;
- Navigation legacy deletion;
- Users legacy Source/Projection deletion;
- Manager browser WORKSPACE/IndexedDB;
- Manager publication/verification/history migration;
- semántica final Users/Profiles/Access;
- bootstrap root contract;
- Profiles Source/Projection separation.

## Refinamiento de no-op publish

La formulación histórica “workspace funcionalmente equivalente a current => no crear versión” queda refinada.

Contrato vigente:

```text
SourceStore.publish(PublishRequest válido)
→ crea una publicación Source

no-op por equivalencia funcional
→ decisión del consumidor antes de invocar publish
```

`SourceReleaseId` identifica publicación y no contenido.
Dos publicaciones con el mismo `content_hash` siguen siendo releases distintas.

Users Canonical Source confirma esta semántica: un mismo `UsersConfigurationCatalog` puede volver a publicarse y producir otra `SourceReleaseId`.

## Manager root Projection cutover

Contrato congelado:

```text
ConfigurationLifecycleWorkflow
    get_current_projection_target() -> ProjectionTarget | None
    project(target: ProjectionTarget) -> ProjectionExecutionResult

ProjectionExecutionResult
    target: ProjectionTarget
```

Invariantes:
- la selección de current puede ocurrir antes de ejecutar;
- una vez seleccionado, `project(target)` transporta exactamente ese target;
- el coordinator no vuelve a consultar current;
- Project callback selecciona target server-side inmediatamente antes de ejecutar;
- browser state no determina la identidad ejecutable;
- Source puede avanzar después de selección sin invalidar el target ya congelado;
- target histórico explícito no se sustituye por current;
- el signal observable conserva exact-release identity;
- no existe shim release/string ni coordinator paralelo.

Estado:

```text
MANAGER-ROOT-CANONICAL-CUTOVER  CLOSED / VERIFIED / CURRENT
```

Checkpoint:

```text
moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
```

La formulación anterior “Manager productive coordinator cutover” queda refinada. Los contratos administrativos de publicación/verificación/history que todavía usan revisiones textuales no forman parte de este cierre.

## Manager browser WORKSPACE

Dirección congelada:

```text
dcc.Store(memory)   = estado activo de sesión
IndexedDB           = persistencia browser del WORKSPACE
SourceStore / Blob  = autoridad durable publicada
ProjectionStore     = proyección activa durable
```

IndexedDB:
- no es autoridad;
- no sustituye Source;
- no requiere gzip/base64 inicialmente;
- se integra mediante JavaScript dedicado + `clientside_callback`;
- su pérdida sólo puede afectar trabajo no publicado.

Implementación: `PLANNED`.

## Users / Profiles / Access

Dirección congelada:

```text
Profiles MUST NOT require Access.
Access MAY consume/extend Profiles.
```

Profiles es capability genérica Atlanticus.
ADA Access es una extensión/consumer específica de ADA.

No incorporar permisos específicos de ADA dentro del Profile genérico.

Los sub-hitos siguientes están `CLOSED / VERIFIED / CURRENT`:

```text
USERS-STORAGE-TOPOLOGY
COSMOS-USERS-RUNTIME-ADAPTER
USERS-RUNTIME-PROJECTION-BOUNDARY
USERS-CANONICAL-SOURCE-1
USERS-CANONICAL-PROJECTION-2
MANAGER-ROOT-CANONICAL-CUTOVER
PROFILES-DOMAIN-EXTRACTION
```

`PROFILES-DOMAIN-EXTRACTION` congela ownership físico, no semántica final:
- Profiles core propio;
- Profiles no depende de Users;
- Users depende one-way de Profiles;
- imports consumidores migrados;
- namespace viejo eliminado sin shim;
- `ProfilesDefinitionError` propio.

Esto no implica cierre de la frontera completa Users / Profiles / ADA Access ni de los cutovers administrativos/runtime restantes.

Contratos congelados adicionales:
- writer Managed snapshot-level separado de runtime store/reader;
- Pending→Resolved por mismo id/partition;
- retirement durable sin delete;
- re-add a present;
- CAS/ETag sin blind upsert;
- Managed disabled no requiere profile histórico;
- Users canonical Source sobre Source Core;
- Users canonical Projection exact-release sobre Projection Core;
- legacy content revision no equivale a Source release identity;
- Manager root Project transporta exact target sin string shim;
- Profiles core no puede adquirir dependencia sobre Users ni ADA Access para resolver la siguiente semántica.

Permanece `OPEN` antes de cerrar la frontera completa:
- contenido del DOCX histórico `Atlanticus_ADA_Usuarios_Perfiles_Acceso_Arquitectura_2026-09-10.docx`;
- semántica final de root/guest/local/administrator;
- representación Guest en runtime;
- bootstrap identity contract de root;
- separación contractual Users/Profiles;
- Profiles Source/Projection;
- validación cross-domain final;
- service key `atlanticus.web.users.profiles`;
- orphan prevention cuando un Profile deja de estar proyectado.

Siguiente foco aislado:

```text
PROFILES-BASELINE-SEMANTICS  PLANNED / NEXT
```

Dirección acordada pero todavía no frozen como contrato implementado:
- `root` bootstrap/platform, fuera de Profiles y de asignación Users normal;
- `guest` baseline reservado Pending/unresolved;
- John/Jane identidades locales con colores estáticos;
- perfiles funcionales como `administrator` pertenecen a Profiles proyectados;
- Managed Users normales referencian perfiles funcionales proyectados por `profile_key`.

La recomendación previa:

```text
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE  NEXT
```

queda `SUPERSEDED AS NEXT` y permanece `PLANNED` para un incremento posterior. No introducir shim `SourceReleaseId <-> str`.

## SharePoint

SharePoint aparece en contratos históricos de Manager/Alarm y en adapters Source legacy.

Blob parity/recovery ya no bloquea el retiro, pero los adapters no se eliminan automáticamente.

Clasificar:
- semántica reusable → conservar;
- storage binding SharePoint → candidato a `SUPERSEDED` durante la migración del consumidor;
- Power Automate Source pipeline → objetivo de retiro durante la migración correspondiente.

Cuando una migración autorice retiro, el incremento debe enumerar rutas exactas a borrar y validar gates después de la eliminación.

## Entrega

La prioridad es integración vertical hacia un entregable usable.

No optimizar roadmap por orden histórico de incrementos si existe un camino más corto y defendible al primer producto integrado.

## ADA Command Center

- Command Center Web propia: `DECIDED / NOT YET IMPLEMENTED`.
- Alarm Engine pertenece funcionalmente a Command Center: `CURRENT`.
- Command Center no authoring de Tools: `CURRENT DIRECTION`.
- Tool topology se consume confirmada/read-only: `FROZEN SEMANTICS`.
- Configuration incluye Alarm Rules + Message Catalog: `CURRENT DIRECTION`.
- Entra ID + Navigation + Profiles: `CURRENT DIRECTION`.
- Generic Actions, User Activity y app/session auto-refresh: `OUT OF SCOPE INITIAL`.
- Live Projection != Management Projection != History/Analytics purpose.
- Dashboard debe producir conclusiones trazables, no métricas decorativas.

## Web Platform / Deployment

- Profiles es capability propia: `CURRENT / IMPLEMENTED + VALIDATED`.
- Profiles puede operar sin Users ni ADA Access: `FROZEN OWNERSHIP/DIRECTION`.
- Users depende one-way de Profiles: `CURRENT / IMPLEMENTED + VALIDATED`.
- Navigation y User Activity conservan fronteras propias: `CURRENT DIRECTION`.
- ADA Access puede consumir/extender Profiles mediante composición ADA: `FROZEN DIRECTION`.
- Cross-capability binding pertenece a composition/adapters: `CURRENT DIRECTION`.
- ADA Manager Users→Navigation direct coupling debe retirarse: `IDENTIFIED GAP`.
- User Activity debe conservar historia ordenada por página/visita: `CURRENT DIRECTION`.
- User Activity TTL = 24 h / 86400 s: `CURRENT`.
- No guardar cada heartbeat como historia: `CURRENT DIRECTION`.
- Dashboard agrega datos sin convertir producers en dependencias mutuas: `CURRENT DIRECTION`.
- Web es startup/resource/projection orchestrator: `CURRENT DIRECTION`.
- Web no es runtime coordinator de Backend jobs: `CURRENT`.
- Web debe existir sin datos/backend y con integrations opcionales ausentes: `CURRENT DIRECTION`.
- Cosmos database puede crearse localmente por bootstrap Web: `CURRENT DIRECTION`.
- Cosmos database productiva debe preexistir; Web no la crea: `CURRENT DIRECTION`.
- Web prepara/valida containers productivos dentro de permisos: `CURRENT DIRECTION`.
- Partition key/TTL mismatch nunca se corrige silenciosamente: `CURRENT`.
- Backend no repite provisioning validation en cada job execution: `CURRENT DIRECTION`.
- Orden de despliegue de aplicación: Web → preparación/proyección → Backend: `CURRENT DIRECTION`.
- Retirar `is_local → full Manager access`: `CURRENT DIRECTION`.
- Crear superficie pre-Manager independiente de Users/Profile projection: `CURRENT DIRECTION`.
- Superficie pre-Manager no implica mutación anónima en producción: `CURRENT`.
- Projection bootstrap debe ordenar por dependencias declaradas, no lista global rígida: `CONTRACT DESIGN`.

## KPI repair / reprocessing

- El pipeline KPI necesita reproceso del estado current sin esperar data nueva: `CURRENT DIRECTION`.
- No usar una flag genérica `DEBUG_MODE`: `CURRENT DIRECTION`.
- Flags de reproceso son por proceso y default false: `PROPOSED`.
- KPI Runtime puede reevaluar el mismo source/committed watermark: `PROPOSED`.
- Same watermark no autoriza overwrite durable conflict: `CURRENT`.
- Latest Delivery puede republicar current ignorando checkpoint current: `PROPOSED`.
- Historian repair reconstruye desde durable evaluations hasta committed: `PROPOSED`.
- Timeseries Delivery puede republicar current desde Historian authority: `PROPOSED`.
- Reprocess jamás permite watermark regression: `CURRENT`.
- Lease/fencing/cancellation permanecen activos: `CURRENT`.

## Bootstrap closure

- Canonical design bootstrap closes at Baseline 1.0.
- Open items remain explicit and do not justify more generic architecture before product execution.
- First Tool: `Operaciones Integradas`.
- Second Tool: `Mina`.
- Command Center analytics is deferred until data sufficiency tests.
- Base projections are independent; only derived resolutions express dependencies.
- Containers/resource inventory remains pending until Tool/component trace is complete.
- Pre-Manager surface is a real authenticated Login/Bootstrap Console.
- Backend and frontend generators target distribution-ready artifacts; Atlanticus does not own corporate DevOps pipeline implementation.
- Component validation scripts continue; a master integrity gate aggregates them.
- Cosmos and Storage are primary supporting services and should be declarable in distribution contracts.
- env.detail becomes explanatory documentation, not just example values.
- Stable components require real READMEs.
- Loaders are product requirements.
- ADA Manager Component External Links use stable Tool/Component identity, JS-controlled popover and warmup/cache.
- University use cases are pedagogical artifacts, not tests.
