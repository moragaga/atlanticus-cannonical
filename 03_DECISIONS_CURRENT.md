# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

| Decisión | Estado |
|---|---|
| Python 3.14.7 | DECIDED / NOT YET IMPLEMENTED GLOBALLY |
| `python:3.14.7-slim-trixie` | DECIDED / NOT YET IMPLEMENTED GLOBALLY |
| `uv`, no pip normal | CURRENT |
| `backend/` representa backend jobs; no todo Python server-side | CURRENT |
| Server-side Python con responsabilidad Web pertenece a `web/` | CURRENT |
| Connectivity es dual-use y no adquiere ownership funcional | CURRENT |
| Definir contratos antes que consumidores | CURRENT |
| Cutover raíz limpio, sin shim/legacy temporal nuevo | CURRENT |
| Read compatibility durable histórica puede preservarse para exact-release replay | CURRENT REFINEMENT |

## Storage / Users durable

| Decisión | Estado |
|---|---|
| Web Storage Topology vive en `web/capabilities/storage/topology` | CURRENT / IMPLEMENTED + VALIDATED |
| `StorageResourceContract` es neutral, sin secretos/SDK/I/O | FROZEN |
| `users.runtime` es el único recurso durable runtime Users confirmado | FROZEN |
| `users.runtime` usa Cosmos, physical `users-runtime`, partition `/id`, TTL `None` | FROZEN |
| Pending y Managed comparten `users.runtime` | FROZEN |
| Users runtime durable data no expira automáticamente por TTL | FROZEN |
| `CosmosUsersRuntimeStore` implementa runtime store + pending reader | FROZEN |
| `observe()` es create-only + conflict reread, nunca blind upsert | FROZEN |
| Managed writer es snapshot-level separado de runtime reader/store | FROZEN |
| Pending→Resolved conserva id/partition | FROZEN |
| Managed removal = disabled + retired, no delete | FROZEN |
| Re-add restaura `managed_state=present` | FROZEN |
| Managed updates usan ETag/CAS | FROZEN |

## Source / Projection

| Decisión | Estado |
|---|---|
| Source genérico pertenece a `web/capabilities/source` | CURRENT |
| Projection exact-release pertenece a `web/capabilities/projection/core` | CURRENT |
| Release identity != content hash | FROZEN |
| Dos releases pueden compartir content hash | FROZEN |
| Source current nunca lo determina Cosmos | FROZEN |
| Projection target = `SourceKey + SourceReleaseRef` | FROZEN |
| `project(target)` no relee current | FROZEN |
| `CURRENT / OUTDATED` compara release identity | FROZEN |
| Retry conserva exact target | FROZEN |
| `UsersConfigurationBundle.revision` no equivale a `SourceReleaseId` | FROZEN |
| No introducir shim `SourceReleaseId <-> str` | FROZEN |

## Profiles ownership

| Decisión | Estado |
|---|---|
| Profiles core vive en `web/capabilities/profiles/core` | CURRENT |
| Profiles no depende de Users | FROZEN OWNERSHIP |
| Users depende one-way de Profiles | FROZEN OWNERSHIP |
| Profiles no depende de ADA Access | FROZEN OWNERSHIP |
| `atlanticus.web.users.profiles` está eliminado sin shim | FROZEN CUTOVER |
| `ProfileCatalog` sólo contiene Profiles explícitos | FROZEN / IMPLEMENTED + VALIDATED |
| `ProfileCatalog()` significa catálogo vacío | FROZEN / IMPLEMENTED + VALIDATED |
| Profiles core no reserva Local/Admin/Guest/Root como system profiles | FROZEN / IMPLEMENTED + VALIDATED |
| `ProfilesConfiguration` es el contrato durable Profiles-owned | FROZEN / IMPLEMENTED + VALIDATED |
| `ProfilesConfiguration` no implica Source/Projection independiente | FROZEN FOR CURRENT BASELINE |
| No inferir `profiles.runtime` | FROZEN |

## Users canonical contract

| Decisión | Estado |
|---|---|
| `UsersConfiguration` contiene sólo Managed Users | FROZEN / IMPLEMENTED + VALIDATED |
| Users own uniqueness: id, non-null email, `(issuer, subject_id)` | FROZEN / IMPLEMENTED + VALIDATED |
| `UsersProfilesConfiguration` es la composición canónica cross-contract | FROZEN / IMPLEMENTED + VALIDATED |
| Cross-contract exige Administrator explícito | FROZEN / IMPLEMENTED + VALIDATED |
| `guest` y `local` no pueden configurarse como Profiles funcionales | FROZEN / IMPLEMENTED + VALIDATED |
| Todo Managed User, enabled o disabled, debe referenciar un Profile existente | FROZEN / IMPLEMENTED + VALIDATED |
| Orphan references se rechazan en la composición canónica | CLOSED / IMPLEMENTED + VALIDATED |

## Canonical Source / Projection split

| Decisión | Estado |
|---|---|
| Una exact Source release contiene Users + Profiles del mismo snapshot | FROZEN / IMPLEMENTED + VALIDATED |
| Resource Users = `users/configuration.json.gz` | FROZEN |
| Resource Profiles = `profiles/configuration.json.gz` | FROZEN |
| Users Source write schema = `2` | CURRENT / IMPLEMENTED + VALIDATED |
| Profiles resource write schema = `1` | CURRENT / IMPLEMENTED + VALIDATED |
| `published_by` permanece en resource Users | CURRENT |
| No crear segundo Source/coordinator para Profiles | FROZEN FOR CURRENT BASELINE |
| Source Users schema `1` se puede leer y normalizar | FROZEN COMPATIBILITY |
| Nuevas publicaciones no escriben schema Users `1` | FROZEN |
| `ProjectionRecord[UsersConfigurationCatalog]` como payload canónico | SUPERSEDED BY UCS-1 |
| `ProjectionRecord[UsersProfilesConfiguration]` es payload canónico vigente | FROZEN / IMPLEMENTED + VALIDATED |
| Cosmos Users configuration projection write schema = `2` | CURRENT / IMPLEMENTED + VALIDATED |

## Admin composition

| Decisión | Estado |
|---|---|
| Payload de authoring canónico = `UsersProfilesConfiguration` | FROZEN / IMPLEMENTED + VALIDATED |
| No crear aggregate admin durable mixto nuevo | FROZEN |
| `UsersProfilesAdminDraft` conserva exact `SourceSnapshot` | FROZEN / IMPLEMENTED + VALIDATED |
| Draft `revision` = SHA-256 del payload canónico actual | FROZEN / IMPLEMENTED + VALIDATED |
| Draft `base_payload_revision` conserva la BASE local | FROZEN / IMPLEMENTED + VALIDATED |
| Draft creado nace limpio (`revision == base_payload_revision`) | FROZEN / IMPLEMENTED + VALIDATED |
| `has_local_changes` depende de revisiones locales, no de Source | FROZEN / IMPLEMENTED + VALIDATED |
| `with_configuration(...)` preserva BASE + exact Source snapshot | FROZEN / IMPLEMENTED + VALIDATED |
| `rebase(...)` adopta snapshot exacto nuevo y hace current revision = BASE | FROZEN / IMPLEMENTED + VALIDATED |
| Draft local revision no equivale a `SourceReleaseId`, `content_hash` ni token | FROZEN |
| Draft document schema vigente = `2` | CURRENT / IMPLEMENTED + VALIDATED |
| Draft schema `1` no se migra mediante parser/adapter | FROZEN CLEAN CUTOVER |
| Administrator no se elimina | FROZEN / IMPLEMENTED + VALIDATED |
| Profile edit preserva key | FROZEN / IMPLEMENTED + VALIDATED |
| Profile referenciado requiere replacement explícito antes de delete | FROZEN / IMPLEMENTED + VALIDATED |
| Reassign + delete ocurre como una sola transformación | FROZEN / IMPLEMENTED + VALIDATED |
| Alta Managed canónica parte de `PendingUserRecord` | FROZEN / IMPLEMENTED + VALIDATED |
| Identidad `(issuer, subject_id)` de Managed existente es inmutable | FROZEN / IMPLEMENTED + VALIDATED |
| Users admin publish usa exact `SourceSnapshot`, `ConcurrencyToken` y `basis_release` | FROZEN / IMPLEMENTED + VALIDATED |

## Manager

| Decisión | Estado |
|---|---|
| Manager posee shell administrativo propio | CURRENT |
| ADA Generic posee shell operacional | CURRENT |
| Manager root Project usa `ProjectionTarget` exacto | FROZEN / IMPLEMENTED + VALIDATED |
| Browser state no es autoridad de target ejecutable | FROZEN |
| Manager publication/verification/history textual no se reinterpretan como release identity | FROZEN |
| `ExactSourcePublicationWorkflow` es extensión opt-in separada del workflow legacy | FROZEN / IMPLEMENTED + VALIDATED |
| Exact-source workflow expone `SourceSnapshot`, no `source_revision: str` | FROZEN / IMPLEMENTED + VALIDATED |
| `ExactSourcePublicationResult` conserva `PublishResult` tipado | FROZEN / IMPLEMENTED + VALIDATED |
| Coordinator exact-source compara snapshot completo antes de publicar | FROZEN / IMPLEMENTED + VALIDATED |
| CAS autoritativo permanece en Source/workflow | FROZEN |
| Workflows legacy no están obligados a implementar exact-source | FROZEN COMPATIBILITY |

## Web compositions

| Decisión | Estado |
|---|---|
| Cross-capability binding puede vivir en `web/compositions` cuando ninguna capability debe poseer la otra | CURRENT |
| `navigation-activity` conecta Navigation con `ActivityRouteResolver`; Activity sigue siendo owner del tracking | CURRENT / PREEXISTING |
| `users-manager` conecta Users Configuration con el protocolo exact-source de Manager | CURRENT / IMPLEMENTED + VALIDATED |
| Manager no depende de Users por el wiring exact-source | FROZEN OWNERSHIP |
| Users Configuration no depende de Manager por el wiring exact-source | FROZEN OWNERSHIP |
| `UsersManagerExactSourceWorkflow` no posee draft, UI, projection ni service registration del host | FROZEN |
| `compositions/` no es un cajón general ni una capa obligatoria | FROZEN DIRECTION |

## Users ↔ Manager exact-source

| Decisión | Estado |
|---|---|
| Adapter/composition exact-source Users↔Manager | CLOSED / VERIFIED / CURRENT |
| Payload del adapter se revalida con `UsersProfilesConfiguration.from_document(...)` | FROZEN / IMPLEMENTED + VALIDATED |
| Actor se obtiene mediante `UsersAuditActorProvider` | FROZEN / IMPLEMENTED + VALIDATED |
| Audit timestamp usa la release confirmada por Source | FROZEN / IMPLEMENTED + VALIDATED |
| Adapter retorna `ExactSourcePublicationResult` | FROZEN / IMPLEMENTED + VALIDATED |
| Adapter hace `rebase()` del draft | SUPERSEDED / FORBIDDEN |
| Adapter registra por sí mismo servicios del host | SUPERSEDED / FORBIDDEN |
| Productive ADA Users workflow ya usa exact-source | PLANNED / NOT YET IMPLEMENTED |
| Productive host todavía registra `UsersManagerWorkflowAdapter` legacy | CURRENT LEGACY |

## Legacy Users Configuration

| Decisión | Estado |
|---|---|
| `UsersConfigurationCatalog` sigue existiendo en camino administrativo productivo legacy | CURRENT LEGACY / NOT CANONICAL WRITE CONTRACT |
| Shape `administrator_* / guest_* / profiles / users` como contrato canónico nuevo | SUPERSEDED BY UCS-1 |
| No adaptar nuevo admin payload canónico de vuelta a `UsersConfigurationCatalog` | FROZEN |
| Migración de callbacks/layout/browser store | PLANNED / NEXT |
| Productive exact-source service cutover | PLANNED |
| Eliminación legacy | BLOCKED |

## Pending / Guest / Administrator

| Decisión | Estado |
|---|---|
| Pending pertenece a Users, no Profiles | FROZEN / IMPLEMENTED + VALIDATED |
| Pending `EffectiveUser.profile is None` | FROZEN / IMPLEMENTED + VALIDATED |
| Guest no es Profile runtime ni Profile durable funcional nuevo | FROZEN / IMPLEMENTED + VALIDATED |
| Administrator es Profile funcional normal y explícito | FROZEN / IMPLEMENTED + VALIDATED |
| Managed Users referencian Profile funcional por `profile_key` | FROZEN / IMPLEMENTED + VALIDATED |

## Root bootstrap

| Decisión | Estado |
|---|---|
| Root pertenece a Identity/bootstrap, no Profiles | FROZEN / IMPLEMENTED + VALIDATED |
| Root no es Managed User ni Profile | FROZEN / IMPLEMENTED + VALIDATED |
| Root match = exact `issuer + subject_id` con policy enabled | FROZEN / IMPLEMENTED + VALIDATED |
| Fuente física Root policy y Entra claim mapping | OPEN / UNVERIFIED |

## Local / John / Jane

| Decisión | Estado |
|---|---|
| Local/John/Jane no son Profiles funcionales | FROZEN BOUNDARY |
| No reintroducirlos como system `ProfileDefinition` | FROZEN |
| Contrato runtime/ownership final | OPEN |

## Service composition

| Decisión | Estado |
|---|---|
| Users WebModule registra sólo servicios Users | FROZEN / IMPLEMENTED + VALIDATED |
| `PROFILE_CATALOG_SERVICE_KEY = "atlanticus.web.users.profiles"` | SUPERSEDED / REMOVED |
| `create_users_module(runtime, profiles)` | SUPERSEDED |
| `create_users_module(runtime)` | CURRENT / IMPLEMENTED + VALIDATED |

## Status de hitos

```text
PROFILES-DOMAIN-EXTRACTION                    CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS                   CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION                     CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT                CLOSED / VERIFIED / CURRENT
ADMIN-COMPOSITION-BACKEND                     CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY                 CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-DRAFT-BASELINE-SEMANTICS CLOSED / VERIFIED / CURRENT
USERS-MANAGER-EXACT-SOURCE-COMPOSITION        CLOSED / VERIFIED / CURRENT
USERS-PROFILES-DOMAIN-SEPARATION              IN PROGRESS
USERS-PROFILES-ADMIN-COMPOSITION              IN PROGRESS
ADMIN-UI-DRAFT-CUTOVER                        PLANNED / NEXT
USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER PLANNED
USERS-RUNTIME-CANONICAL-CUTOVER               PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE        PLANNED
USERS-ADMIN-CANONICAL-MIGRATION               PLANNED
```

## Refinamientos / superseded de este cierre

1. “Draft canónico schema 1 con sólo `revision`”
   → `SUPERSEDED`: schema 2 agrega `base_payload_revision` y semántica clean/dirty/rebase.

2. “USERS-MANAGER-EXACT-SOURCE-WIRING” como un único hito binario
   → `REFINED` en dos fronteras:
   - adapter/composition exact-source: CLOSED;
   - productive host cutover: PLANNED.

3. “El adapter exact-source debe vivir dentro de Users o Manager”
   → `SUPERSEDED`: vive en `web/compositions/users-manager`.

4. “El workflow exact-source debería rebasar el draft”
   → `SUPERSEDED`: caller/session posee el draft y hace rebase después del éxito.

5. “Users exact-source ya está productivamente conectado porque existe el adapter”
   → `SUPERSEDED`: el host ADA todavía registra `UsersManagerWorkflowAdapter` legacy.

6. “`compositions/` apareció con Users↔Manager”
   → `SUPERSEDED`: `navigation-activity` ya era una composition preexistente.

## Siguiente decisión de ejecución

Único foco recomendado:

```text
ADMIN-UI-DRAFT-CUTOVER
```

No mezclar en ese incremento runtime provenance, Python migration, Root physical configuration ni legacy deletion global.
