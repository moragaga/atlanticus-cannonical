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
| `assignable()` deja de ser contrato de `ProfileCatalog` | SUPERSEDED / REMOVED |
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

## Canonical Source contract split

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
| Legacy Administrator colors → explicit Administrator Profile durante normalization | FROZEN COMPATIBILITY |
| Legacy Guest fields no crean Profile funcional | FROZEN COMPATIBILITY |

## Canonical Projection contract split

| Decisión | Estado |
|---|---|
| `ProjectionRecord[UsersConfigurationCatalog]` como payload canónico | SUPERSEDED BY UCS-1 |
| `ProjectionRecord[UsersProfilesConfiguration]` es payload canónico vigente | FROZEN / IMPLEMENTED + VALIDATED |
| Cosmos Users configuration projection write schema = `2` | CURRENT / IMPLEMENTED + VALIDATED |
| Cosmos projection schema `1` se puede leer y normalizar | FROZEN COMPATIBILITY |
| same exact release + same payload = idempotent | FROZEN |
| same exact release + different payload = invariant error | FROZEN |
| new exact release usa CAS/ETag para reemplazo activo | FROZEN |
| concurrent same-target winner = idempotent success | FROZEN |
| concurrent different target = conflict | FROZEN |

## Admin composition

| Decisión | Estado |
|---|---|
| El payload de authoring canónico es `UsersProfilesConfiguration`, no un aggregate admin nuevo | FROZEN / IMPLEMENTED + VALIDATED |
| `UsersProfilesAdminDraft` conserva `SourceSnapshot` exacto | FROZEN / IMPLEMENTED + VALIDATED |
| Draft `revision` = SHA-256 del JSON canónico del payload y sólo identifica el draft local | FROZEN / IMPLEMENTED + VALIDATED |
| Draft local revision no equivale a `SourceReleaseId` ni `content_hash` | FROZEN |
| Nuevo draft usa `document_type=atlanticus_users_profiles_admin_draft`, schema `1` | CURRENT / IMPLEMENTED + VALIDATED |
| Parser del nuevo draft no acepta shape legacy | FROZEN CLEAN CUTOVER DIRECTION |
| Administrator no se elimina | FROZEN / IMPLEMENTED + VALIDATED |
| Administrator se edita por operación dedicada; key y label se preservan en el contrato actual | CURRENT / IMPLEMENTED + VALIDATED |
| Profile funcional creado deriva key desde label | CURRENT / IMPLEMENTED + VALIDATED |
| Profile key es inmutable durante edición | FROZEN / IMPLEMENTED + VALIDATED |
| Profile referenciado requiere replacement explícito antes de delete | FROZEN / IMPLEMENTED + VALIDATED |
| Reassign + delete ocurre como una sola transformación validada | FROZEN / IMPLEMENTED + VALIDATED |
| Reassign aplica también a Users disabled | FROZEN / IMPLEMENTED + VALIDATED |
| Alta Managed canónica parte de `PendingUserRecord` | FROZEN / IMPLEMENTED + VALIDATED |
| Identidad `(issuer, subject_id)` de Managed existente es inmutable | FROZEN / IMPLEMENTED + VALIDATED |
| No crear upsert genérico de Managed que invente identidad | FROZEN |
| Users admin publish usa exact `SourceSnapshot`, `ConcurrencyToken` y `basis_release` | FROZEN / IMPLEMENTED + VALIDATED |

## Pending / Guest

| Decisión | Estado |
|---|---|
| Pending pertenece a Users, no Profiles | FROZEN / IMPLEMENTED + VALIDATED |
| Pending `EffectiveUser.profile is None` | FROZEN / IMPLEMENTED + VALIDATED |
| Pending siempre enabled | FROZEN / IMPLEMENTED + VALIDATED |
| Pending no puede ser Local | FROZEN / IMPLEMENTED + VALIDATED |
| Pending avatar no acepta overrides | FROZEN / IMPLEMENTED + VALIDATED |
| Pending visual actual = `#FF5722` / `#FFFFFF` | FROZEN FOR CURRENT BASELINE |
| Guest no es Profile runtime | FROZEN / IMPLEMENTED + VALIDATED |
| Guest no existe como Profile durable funcional nuevo | FROZEN / IMPLEMENTED + VALIDATED |
| Resolved User requiere Profile funcional | FROZEN / IMPLEMENTED + VALIDATED |
| Resolved User no puede usar `profile_key="guest"` | FROZEN / IMPLEMENTED + VALIDATED |

## Administrator / functional Profiles

| Decisión | Estado |
|---|---|
| Administrator es Profile funcional normal | FROZEN / IMPLEMENTED + VALIDATED |
| Administrator canónico es `ProfileDefinition` explícito | FROZEN / IMPLEMENTED + VALIDATED |
| Operator/Viewer/custom son Profiles funcionales | FROZEN DIRECTION |
| Managed Users referencian Profile funcional por `profile_key` | FROZEN / IMPLEMENTED + VALIDATED |
| Guest/Local no se materializan en runtime Profile catalog | FROZEN / IMPLEMENTED + VALIDATED |

## Legacy Users Configuration

| Decisión | Estado |
|---|---|
| Combined `UsersConfigurationCatalog` sigue existiendo en camino administrativo productivo legacy | CURRENT LEGACY / NOT CANONICAL WRITE CONTRACT |
| Shape `administrator_* / guest_* / profiles / users` como contrato canónico nuevo | SUPERSEDED BY UCS-1 |
| `guest_*` puede round-trip en contrato legacy | CURRENT LEGACY |
| Nuevas Source publications omiten `guest_*` | FROZEN / IMPLEMENTED + VALIDATED |
| No adaptar el nuevo admin payload canónico de vuelta a `UsersConfigurationCatalog` | FROZEN |
| Migración de callbacks/layout/browser store | PLANNED / NEXT |
| Eliminación legacy | BLOCKED |

## Manager

| Decisión | Estado |
|---|---|
| Manager posee shell administrativo propio | CURRENT |
| ADA Generic posee shell operacional | CURRENT |
| Manager root Project usa `ProjectionTarget` exacto | FROZEN / IMPLEMENTED + VALIDATED |
| Project callback selecciona current server-side | FROZEN |
| Browser state no es autoridad de target ejecutable | FROZEN |
| Manager publication/verification/history textual no se reinterpretan como release identity | FROZEN REFINEMENT |
| `ExactSourcePublicationWorkflow` es extensión opt-in separada del workflow legacy | FROZEN / IMPLEMENTED + VALIDATED |
| Exact-source workflow expone `SourceSnapshot`, no `source_revision: str` | FROZEN / IMPLEMENTED + VALIDATED |
| `ExactSourcePublicationResult` conserva `PublishResult` tipado | FROZEN / IMPLEMENTED + VALIDATED |
| Coordinator exact-source compara snapshot completo antes de publicar | FROZEN / IMPLEMENTED + VALIDATED |
| Exact-source failure + Source cambiado se adjudica como source conflict | FROZEN / IMPLEMENTED + VALIDATED |
| Workflows legacy no están obligados a implementar exact-source | FROZEN COMPATIBILITY |
| Users↔Manager exact-source wiring | PLANNED |
| Manager WORKSPACE futuro: `dcc.Store(memory)` + IndexedDB | DECIDED / NOT YET IMPLEMENTED |
| IndexedDB no es Source authority | FROZEN |

## Root bootstrap

| Decisión | Estado |
|---|---|
| Root pertenece a Identity/bootstrap, no Profiles | FROZEN / IMPLEMENTED + VALIDATED |
| Root no es Managed User | FROZEN / IMPLEMENTED + VALIDATED |
| Root no es Profile | FROZEN / IMPLEMENTED + VALIDATED |
| Root match = exact `issuer + subject_id` con policy enabled | FROZEN / IMPLEMENTED + VALIDATED |
| `provider_key` no participa del Root match | FROZEN / IMPLEMENTED + VALIDATED |
| Root access = READY + `bootstrap_root=True` + `user_id=None` | FROZEN / IMPLEMENTED + VALIDATED |
| Non-match/disabled cae al fallback normal | FROZEN / IMPLEMENTED + VALIDATED |
| Access session contract usa `_atlanticus_access_snapshot_v2` | CURRENT |
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
| Dependencia semántica `UsersAccessResolver -> ProfileCatalog` permanece válida | CURRENT |

## Status de hitos

```text
PROFILES-DOMAIN-EXTRACTION                CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS               CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION                 CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT            CLOSED / VERIFIED / CURRENT
USERS-PROFILES-DOMAIN-SEPARATION          IN PROGRESS
USERS-PROFILES-ADMIN-COMPOSITION          IN PROGRESS
ADMIN-COMPOSITION-BACKEND                 CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY             CLOSED / VERIFIED / CURRENT
ADMIN-UI-DRAFT-CUTOVER                    PLANNED / NEXT
USERS-MANAGER-EXACT-SOURCE-WIRING         PLANNED
USERS-RUNTIME-CANONICAL-CUTOVER           PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE    PLANNED
USERS-ADMIN-CANONICAL-MIGRATION           PLANNED
```

## Refinamientos / superseded

1. “`ProfileCatalog` preserva Local + Administrator + Guest”
   → `SUPERSEDED`: catálogo puro y explícito.

2. “Guest es un Profile runtime base”
   → `SUPERSEDED`: Pending User con `profile=None`; Guest fuera de Profiles runtime.

3. “Root es sólo dirección propuesta fuera de Profiles”
   → `REFINED + IMPLEMENTED` en Identity/bootstrap.

4. “`PROFILE_CATALOG_SERVICE_KEY` sigue vigente en Users”
   → `SUPERSEDED / REMOVED`.

5. “El aggregate durable canónico vigente es siempre `UsersConfigurationCatalog`”
   → `SUPERSEDED BY UCS-1`.

6. “`ProjectionRecord[UsersConfigurationCatalog]` sigue congelado”
   → `SUPERSEDED BY UCS-1`.

7. “Administrator/Guest durable fields deben encontrar ownership dentro del nuevo contrato”
   → `REFINED/CLOSED`: Administrator explícito; Guest sólo legacy/history.

8. “Profile deletion/orphan rule permanece abierta”
   → `REFINED/CLOSED`: delete referenciado exige replacement y la operación canónica reasigna + elimina atómicamente.

9. “Profiles puede requerir Source/Projection propia para separar ownership”
   → `NOT REQUIRED FOR CURRENT BASELINE`.

10. “El admin composition debe crear un nuevo aggregate que posea Users + Profiles”
    → `SUPERSEDED`: authoring usa directamente `UsersProfilesConfiguration`.

11. “Manager necesita reinterpretar `source_revision: str` para publicar exact-source”
    → `SUPERSEDED`: exact-source es un protocolo opt-in con `SourceSnapshot` y `PublishResult` tipados.

12. “Toda publication administrativa de Manager sigue necesariamente por contrato textual legacy”
    → `REFINED`: el camino legacy permanece, pero existe un boundary exact-source opt-in ya implementado.

13. `USERS-PROFILES-ADMIN-COMPOSITION` como trabajo completamente no iniciado
    → `REFINED`: backend composition y Manager exact-source boundary están CLOSED; UI/store y wiring permanecen pendientes.

## Decisiones especializadas preservadas

Las decisiones detalladas de Alarm, Command Center, Source/Blob, Manager y otros dominios permanecen vigentes salvo los refinamientos explicitados arriba.

Este cierre no reabre dominios no relacionados.
