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
| Cutover raíz limpio; no crear shims legacy temporales nuevos | CURRENT |
| Read/import compatibility histórica puede preservarse separada del authoring canónico | CURRENT REFINEMENT |

## Source / Projection

| Decisión | Estado |
|---|---|
| Source genérico pertenece a `web/capabilities/source` | CURRENT |
| Projection exact-release pertenece a `web/capabilities/projection/core` | CURRENT |
| Release identity != content hash | FROZEN |
| Source current nunca lo determina Cosmos | FROZEN |
| Projection target = `SourceKey + SourceReleaseRef` | FROZEN |
| `project(target)` no relee current | FROZEN |
| `CURRENT / OUTDATED` compara release identity | FROZEN |
| Retry conserva exact target | FROZEN |
| No introducir shim `SourceReleaseId <-> str` | FROZEN |
| Restore publica una nueva release; no repunta current | FROZEN |

## Users / Profiles ownership

| Decisión | Estado |
|---|---|
| Profiles no depende de Users | FROZEN OWNERSHIP |
| Users depende one-way de Profiles | FROZEN OWNERSHIP |
| `ProfilesConfiguration` es contrato durable Profiles-owned | FROZEN |
| `UsersConfiguration` contiene sólo Managed Users | FROZEN |
| `UsersProfilesConfiguration` es composición canónica cross-contract | FROZEN |
| Administrator es Profile explícito | FROZEN |
| `guest` y `local` no son Profiles funcionales configurables | FROZEN |
| Todo Managed User debe referenciar Profile existente | FROZEN |
| No inferir `profiles.runtime` | FROZEN |

## Canonical Users Source / Projection

| Decisión | Estado |
|---|---|
| Una exact Source release contiene Users + Profiles del mismo snapshot | FROZEN |
| Resource Users = `users/configuration.json.gz` | FROZEN |
| Resource Profiles = `profiles/configuration.json.gz` | FROZEN |
| Users Source write schema = `2` | CURRENT |
| Profiles resource write schema = `1` | CURRENT |
| Source Users schema `1` puede leerse/normalizarse | FROZEN COMPATIBILITY |
| Nuevas publicaciones no escriben Users schema `1` | FROZEN |
| Projection payload canónico = `UsersProfilesConfiguration` | FROZEN / CURRENT |
| Cosmos projection write schema = `2` | CURRENT |

## Admin composition

| Decisión | Estado |
|---|---|
| Payload de authoring canónico = `UsersProfilesConfiguration` | FROZEN |
| `UsersProfilesAdminDraft` conserva exact `SourceSnapshot` | FROZEN |
| Draft schema vigente = `2` | CURRENT |
| Draft schema `1` no se adapta | FROZEN CLEAN CUTOVER |
| Local revision no equivale a release/hash/token | FROZEN |
| Profile referenciado exige replacement explícito en backend | FROZEN |
| Managed create canónico parte de Pending | FROZEN |
| Identidad Managed existente es inmutable | FROZEN |

## Manager exact-source / exact-projection

| Decisión | Estado |
|---|---|
| Exact Source read, publication, history y Projection son capabilities independientes | FROZEN / IMPLEMENTED |
| Un `ManagerModule` exacto puede tener `workflow_service=None` | FROZEN / IMPLEMENTED |
| No fallback silencioso exact→legacy | FROZEN |
| `ExactSourceReaderWorkflow` devuelve `ExactSourceReadResult` | FROZEN |
| `ExactSourcePublicationWorkflow` usa exact `SourceSnapshot` | FROZEN |
| `ExactSourceHistoryWorkflow` usa `HistoryPage` + `SourceReleaseRef` | FROZEN |
| `ExactProjectionWorkflow` usa status/target/result de `projection/core` | FROZEN |
| Status exacto no se adapta a `Manager ProjectionStatus` legacy | FROZEN |
| Exact History no se adapta a `RevisionHistoryEntry` | FROZEN |
| History y status son fallos/capabilities separados | FROZEN |
| CAS autoritativo permanece en Source/workflow | FROZEN |

## Users exact Manager lifecycle

| Decisión | Estado |
|---|---|
| Users Manager validation exacta/canónica | CLOSED / VERIFIED / CURRENT |
| Users Source read exacto | CLOSED / VERIFIED / CURRENT |
| Users Source publication exacta | CLOSED / VERIFIED / CURRENT |
| Users Projection status exacto | CLOSED / VERIFIED / CURRENT |
| Users Projection exact-target | CLOSED / VERIFIED / CURRENT |
| Users Source History list/read exactos | CLOSED / VERIFIED / CURRENT |
| Users History preview canónico | CLOSED / VERIFIED / CURRENT |
| Historical release se carga como trabajo local sobre BASE current | FROZEN / IMPLEMENTED |
| `UsersManagerWorkflowAdapter` productivo | SUPERSEDED / REMOVED |
| `workflow_service` legacy para Users | SUPERSEDED / NONE |
| Productive Users exact-source cutover como hito pendiente | SUPERSEDED BY FULL EXACT LIFECYCLE |

## Historical load semantics

| Decisión | Estado |
|---|---|
| Abrir una release histórica repunta Source current | SUPERSEDED / FORBIDDEN |
| Historical load conserva current BASE | FROZEN |
| Historical load crea local dirty work | FROZEN |
| Publicar luego del historical load crea nueva release | FROZEN |
| Source release histórica se identifica con `release_id` string aislado | SUPERSEDED / FORBIDDEN |
| Dash transporta release id + published timestamp para reconstruir `SourceReleaseRef` | CURRENT |

## Productive ADA composition

| Decisión | Estado |
|---|---|
| Users admin recibe `UsersProfilesAdministrationService` | CURRENT |
| Users exact Projection se inyecta como `ExactProjectionWorkflow` ya compuesto | CURRENT |
| ADA registra validation/read/history/publication exactos Users | CURRENT |
| ADA recompone stores privados de Users Projection | SUPERSEDED / FORBIDDEN |
| ADA exporta `UsersManagerWorkflowAdapter` | SUPERSEDED / REMOVED |
| Navigation/Tools/KPI legacy adapters permanecen otro frente | CURRENT LEGACY / OPEN |

## Qualification / packaging

| Hallazgo | Estado |
|---|---|
| Current implementation checkpoint | VERIFIED / `384a68fe8fa42263623c95d1d132af2ca54574c8` |
| Focused Manager + Users Configuration + users-manager suite | VERIFIED / 238 passed |
| ADA full suite | VERIFIED / 56 passed, 4 failed |
| Los 4 failures ADA pertenecen a Users | SUPERSEDED / FALSE |
| Los 4 failures ADA pertenecen a legacy Projection adapters no-Users | VERIFIED / BLOCKER FOR GLOBAL ADA GREEN |
| Full Web suite en current checkpoint | UNVERIFIED |
| Docker E2E | UNVERIFIED |
| Python 3.14.7 qualification current checkpoint | UNVERIFIED |
| CI remoto adicional | UNVERIFIED |

## Status de hitos

```text
MANAGER-EXACT-SOURCE-WORKSPACE-CAPABILITIES   CLOSED / VERIFIED / CURRENT
USERS-EXACT-SOURCE-PRODUCTIVE-HOST-CUTOVER    CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-ONLY-MODULE-CONTRACT             CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-PROJECTION-BOUNDARY              CLOSED / VERIFIED / CURRENT
USERS-EXACT-PROJECTION-COMPOSITION             CLOSED / VERIFIED / CURRENT
USERS-EXACT-PROJECTION-HOST-CUTOVER            CLOSED / VERIFIED / CURRENT
USERS-EXACT-PROJECTION-STATUS-CUTOVER          CLOSED / VERIFIED / CURRENT
USERS-EXACT-SOURCE-HISTORY-BOUNDARY            CLOSED / VERIFIED / CURRENT
USERS-EXACT-SOURCE-HISTORY-HOST-UI-CUTOVER    CLOSED / VERIFIED / CURRENT
USERS-EXACT-MANAGER-LIFECYCLE                 CLOSED / VERIFIED / CURRENT

ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT       PLANNED / NEXT
USERS-RUNTIME-CANONICAL-CUTOVER               PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE        PLANNED
DOMAIN-LEGACY-DELETION                        BLOCKED
```

## Refinamientos / superseded

1. “Users exact-source productivo todavía requiere `UsersManagerWorkflowAdapter`”
   → `SUPERSEDED`: el adapter fue eliminado; Users no declara lifecycle legacy.

2. “Exact status debe traducirse al `ProjectionStatus` legacy de Manager”
   → `SUPERSEDED / FORBIDDEN`: Manager presenta el status de `projection/core`.

3. “History exacto debe convertirse a `RevisionHistoryEntry`”
   → `SUPERSEDED / FORBIDDEN`: `HistoryPage` y `SourceReleaseRef` se preservan.

4. “History release puede identificarse sólo por un string revision”
   → `SUPERSEDED`: la lectura exacta usa `SourceReleaseRef`.

5. “History preview Users sigue legacy”
   → `SUPERSEDED`: preview parsea `UsersProfilesConfiguration`.

6. “Cargar History debe restaurar/repoint current”
   → `SUPERSEDED`: sólo carga payload como trabajo local sobre BASE current.

7. “Status y History deben cargarse/fallar como una unidad”
   → `REFINED`: History no degrada un status exacto válido.

8. “El siguiente foco es USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER”
   → `SUPERSEDED`: el lifecycle Users quedó exacto de punta a punta.

9. “Los failures ADA actuales bloquean el cierre Users”
   → `REFINED`: bloquean GREEN global ADA, pero pertenecen a Projection legacy no-Users.

## Siguiente decisión de ejecución

Único foco recomendado:

```text
ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT
```

Alinear exclusivamente:

- Navigation;
- Tools;
- KPI;
- KPI Definitions.

No reabrir Users exact lifecycle.
