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
| Cutover raíz limpio, sin shim/legacy temporal | CURRENT |

## Storage / Users durable

| Decisión | Estado |
|---|---|
| Web Storage Topology vive en `web/capabilities/storage/topology` | CURRENT / IMPLEMENTED + VALIDATED |
| `StorageResourceContract` es neutral, sin secretos/SDK/I/O | FROZEN |
| `users.runtime` es el único recurso durable Users confirmado | FROZEN |
| `users.runtime` usa Cosmos, physical `users-runtime`, partition `/id`, TTL `None` | FROZEN |
| Pending y Managed comparten `users.runtime` | FROZEN |
| Users durable data no expira automáticamente por TTL | FROZEN |
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
| `ProjectionRecord[UsersConfigurationCatalog]` sigue siendo payload canónico Users vigente | FROZEN UNTIL EXPLICIT CUTOVER |
| `UsersConfigurationBundle.revision` no equivale a `SourceReleaseId` | FROZEN |
| No introducir shim `SourceReleaseId <-> str` | FROZEN |

## Manager

| Decisión | Estado |
|---|---|
| Manager posee shell administrativo propio | CURRENT |
| ADA Generic posee shell operacional | CURRENT |
| Manager root Project usa `ProjectionTarget` exacto | FROZEN / IMPLEMENTED + VALIDATED |
| Project callback selecciona current server-side | FROZEN |
| Browser state no es autoridad de target ejecutable | FROZEN |
| Manager publication/verification/history textual no se reinterpretan como release identity | FROZEN REFINEMENT |
| Manager WORKSPACE futuro: `dcc.Store(memory)` + IndexedDB | DECIDED / NOT YET IMPLEMENTED |
| IndexedDB no es Source authority | FROZEN |

## Profiles ownership

| Decisión | Estado |
|---|---|
| Profiles core vive en `web/capabilities/profiles/core` | CURRENT |
| Profiles no depende de Users | FROZEN OWNERSHIP |
| Users depende one-way de Profiles | FROZEN OWNERSHIP |
| Profiles no depende de ADA Access | FROZEN OWNERSHIP |
| `atlanticus.web.users.profiles` está eliminado sin shim | FROZEN CUTOVER |
| Errores Profiles usan `ProfilesDefinitionError` | CURRENT |
| `ProfileCatalog` sólo contiene Profiles explícitos | FROZEN / IMPLEMENTED + VALIDATED |
| `ProfileCatalog()` significa catálogo vacío | FROZEN / IMPLEMENTED + VALIDATED |
| Profiles core no reserva Local/Admin/Guest/Root como system profiles | FROZEN / IMPLEMENTED + VALIDATED |
| `assignable()` deja de ser contrato de `ProfileCatalog` | SUPERSEDED / REMOVED |

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
| Resolved User requiere Profile funcional | FROZEN / IMPLEMENTED + VALIDATED |
| Resolved User no puede usar `profile_key="guest"` | FROZEN / IMPLEMENTED + VALIDATED |

## Administrator / functional Profiles

| Decisión | Estado |
|---|---|
| Administrator es Profile funcional normal | FROZEN / IMPLEMENTED + VALIDATED |
| Operator/Viewer/custom son Profiles funcionales | FROZEN DIRECTION |
| Managed Users referencian Profile funcional por `profile_key` | FROZEN / IMPLEMENTED + VALIDATED |
| Runtime catalog de Users Configuration = Administrator + configured functional Profiles | FROZEN / IMPLEMENTED + VALIDATED |
| Guest/Local no se materializan en runtime Profile catalog | FROZEN / IMPLEMENTED + VALIDATED |
| Sin proyección, `FileUsersProjectionProfileCatalog` está vacío | FROZEN / IMPLEMENTED + VALIDATED |

## Durable Users Configuration

| Decisión | Estado |
|---|---|
| Shape durable actual se preserva durante baseline semantics | FROZEN FOR CURRENT CONTRACT |
| `guest_*` durable fields round-trip pero no crean runtime Guest Profile | FROZEN / IMPLEMENTED + VALIDATED |
| Separación durable Users/Profiles se ejecutará en incremento separado | PLANNED |
| No cambiar schema/source release identity silenciosamente | FROZEN |

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
| No se añadió casefold al match Root | FROZEN |
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

## Status del hito Profiles

```text
PROFILES-DOMAIN-EXTRACTION               CLOSED / VERIFIED / CURRENT
PB-1 PROFILES-SEMANTIC-CORE              CLOSED / VERIFIED / INTEGRATED
PB-2 DIRECT-CONSUMER-RECONCILIATION      CLOSED / VERIFIED / INTEGRATED
PB-3 ROOT-ACCESS-CONTRACT                 CLOSED / VERIFIED / INTEGRATED
PB-4 USERS-CONFIG-RECONCILIATION          ABSORBED BY PB-2 / CLOSED
PB-5 USERS-ADMIN-SEMANTIC-CLEANUP         ABSORBED BY PB-2 / CLOSED
PB-6 PROFILE-SERVICE-COMPOSITION-CLEANUP  CLOSED / VERIFIED / INTEGRATED
PROFILES-BASELINE-SEMANTICS               CLOSED / VERIFIED / CURRENT
```

## Refinamientos / superseded

Quedan reemplazadas o refinadas:

1. “`ProfileCatalog` preserva Local + Administrator + Guest”  
   → `SUPERSEDED` por PB-1: catálogo puro y explícito.

2. “Guest es un Profile runtime base”  
   → `SUPERSEDED` por PB-2: Pending User con `profile=None`.

3. “Root es sólo dirección propuesta fuera de Profiles”  
   → `REFINED + IMPLEMENTED` por PB-3 en Identity/bootstrap.

4. “`PROFILE_CATALOG_SERVICE_KEY` sigue vigente en Users”  
   → `SUPERSEDED / REMOVED` por PB-6.

5. PB-4 y PB-5 como incrementos futuros separados  
   → `ABSORBED BY PB-2`.

6. `USERS-RUNTIME-EXACT-RELEASE-PROVENANCE` como siguiente foco  
   → continúa PLANNED pero deja de ser NEXT; primero `USERS-CONTRACT-SEPARATION`.

## Decisiones especializadas preservadas

Las decisiones detalladas de Alarm, Command Center, Source/Blob, Manager y otros dominios permanecen vigentes en sus documentos canónicos especializados.

Este cierre no las reabre.
