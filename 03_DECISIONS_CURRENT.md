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
| Managed writer hacia `users.runtime` no se inventa dentro de `UsersRuntimeStore`; primero se congela ownership de Projection | CURRENT / NEXT DESIGN |
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
| No crear shim `SourceReleaseId <-> str` para completar la migración Manager | FROZEN |
| No crear un segundo Manager coordinator canónico paralelo | FROZEN |
| Manager browser WORKSPACE persistirá en IndexedDB | DECIDED / NOT YET IMPLEMENTED |
| Manager active workspace en Dash usa `dcc.Store(memory)` | DECIDED / NOT YET IMPLEMENTED |
| IndexedDB no es Source authority | FROZEN |
| Profiles debe funcionar sin Access | FROZEN DIRECTION |
| ADA Access puede consumir/extender Profiles; Profiles no depende de ADA Access | FROZEN DIRECTION |
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
SOURCE-1A.1                       Core + Local                     CLOSED / VERIFIED
SOURCE-1A.2                       Blob                             CLOSED / VERIFIED
Projection                        Exact-release Core               CLOSED / VERIFIED
NAV-SOURCE-PROJECTION-1           Source contracts                 CLOSED / VERIFIED
NAV-SOURCE-PROJECTION-2           Local/Cosmos Projection stores   CLOSED / VERIFIED
NAV-CONSUMER-MIGRATION-A          Runtime consumer                 CLOSED / VERIFIED
NAV-CONSUMER-MIGRATION-B          Administrative consumer          BLOCKED
Manager                           Root canonical cutover           BLOCKED / IN PROGRESS
WEB-STORAGE-TOPOLOGY              Resource contracts               CLOSED / VERIFIED
USERS-STORAGE-TOPOLOGY            users.runtime                    CLOSED / VERIFIED
STORAGE-PREFLIGHT-COSMOS-BRIDGE   Cosmos resource preflight        CLOSED / VERIFIED
COSMOS-USERS-RUNTIME-ADAPTER      Runtime store/reader              CLOSED / VERIFIED
Next                              USERS-RUNTIME-PROJECTION-BOUNDARY PLANNED
```

Implementación actual relevante:

```text
web/capabilities/storage/topology
web/capabilities/storage/cosmos
web/capabilities/users/core
web/capabilities/users/cosmos
web/capabilities/users/configuration
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
- no posee el writer de Projection de Managed Users.

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
- History proviene de Source y no de un contenedor histórico de dominio;
- mismo contenido puede republicarse como una release distinta;
- implementa Projection stores concretos Local/Cosmos;
- runtime usa el `ProjectionStore` canónico;
- administración sigue bloqueada por el contrato productivo legacy de Manager.

Manager:
- `workspace.py` ya modela BASE/SOURCE/WORKSPACE/PROJECTION con `SourceSnapshot`, `ConcurrencyToken`, `basis_release` y `ProjectionTarget`;
- coordinator/workflows productivos siguen usando `source_revision: str`;
- el cutover de raíz no debe introducir adapters temporales ni coordinators paralelos.

No se consideran cerrados por este hito:
- provisioning/validation real de `users.runtime` dentro del lifecycle Web;
- `USERS-RUNTIME-PROJECTION-BOUNDARY` y writer durable de Managed Users;
- otros providers Projection Local/Cosmos concretos por dominio;
- orchestration multi-capability;
- derived resolutions;
- idempotencia provider/domain-level de reprojection;
- Manager root productive cutover;
- Navigation administrative consumer migration;
- Navigation legacy deletion;
- Users/Profiles/Access boundary audit completa.

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

Los sub-hitos `USERS-STORAGE-TOPOLOGY` y `COSMOS-USERS-RUNTIME-ADAPTER` están `CLOSED / VERIFIED`.

Esto no implica cierre de la frontera completa Users / Profiles / ADA Access ni del writer de Users Projection.

Antes de cerrar esa frontera completa todavía se debe auditar:
- implementación actual Users/Profile restante;
- consumidores reales;
- `Atlanticus_ADA_Usuarios_Perfiles_Acceso_Arquitectura_2026-09-10.docx`.

El siguiente foco aislado es `USERS-RUNTIME-PROJECTION-BOUNDARY`: primero contrato/ownership; después implementación.

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

- Users/Profile, Navigation y User Activity deben poder instalarse independientemente: `CURRENT DIRECTION`.
- Profiles debe poder operar sin ADA Access: `FROZEN DIRECTION`.
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
