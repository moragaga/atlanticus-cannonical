# Atlanticus — Current State

Estado: **CURRENT EXECUTION CHECKPOINT**

Corte de implementación:
`moragaga/atlanticus@384a68fe8fa42263623c95d1d132af2ca54574c8`.

Parent inmediato:
`moragaga/atlanticus@b2254450b4543d2422ca8580357b9054b515cd6e`.

## Resumen de estado

```text
WEB-STORAGE-TOPOLOGY                          CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY                        CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE               CLOSED / VERIFIED / CURRENT
COSMOS-USERS-RUNTIME-ADAPTER                  CLOSED / VERIFIED / CURRENT
USERS-RUNTIME-PROJECTION-BOUNDARY             CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-SOURCE-1                      CLOSED / VERIFIED / CURRENT
USERS-CANONICAL-PROJECTION-2                  CLOSED / VERIFIED / CURRENT
MANAGER-ROOT-CANONICAL-CUTOVER                CLOSED / VERIFIED / CURRENT
PROFILES-DOMAIN-EXTRACTION                    CLOSED / VERIFIED / CURRENT
PROFILES-BASELINE-SEMANTICS                   CLOSED / VERIFIED / CURRENT
USERS-CONTRACT-SEPARATION                     CLOSED / VERIFIED / CURRENT
UCS-1 CANONICAL-CONTRACT-SPLIT                CLOSED / VERIFIED / CURRENT
ADMIN-COMPOSITION-BACKEND                     CLOSED / VERIFIED / CURRENT
MANAGER-EXACT-SOURCE-BOUNDARY                 CLOSED / VERIFIED / CURRENT
USERS-PROFILES-ADMIN-DRAFT-BASELINE-SEMANTICS CLOSED / VERIFIED / CURRENT
USERS-MANAGER-EXACT-SOURCE-COMPOSITION        CLOSED / VERIFIED / CURRENT
ADMIN-UI-DRAFT-CUTOVER                        CLOSED / VERIFIED / CURRENT
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

USERS-PROFILES-DOMAIN-SEPARATION              IN PROGRESS
USERS-PROFILES-ADMIN-COMPOSITION              IN PROGRESS

ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT       PLANNED / NEXT
USERS-RUNTIME-CANONICAL-CUTOVER               PLANNED
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE        PLANNED
USERS-ADMIN-CANONICAL-MIGRATION               IN PROGRESS
NAV-CONSUMER-MIGRATION-B                      PLANNED
DOMAIN-LEGACY-DELETION                        BLOCKED
```

`USERS-MANAGER-PRODUCTIVE-EXACT-SOURCE-CUTOVER` queda **SUPERSEDED** por el cierre más preciso de `USERS-EXACT-MANAGER-LIFECYCLE`.

## Plataforma genérica

Atlanticus mantiene fronteras separadas para backend jobs, connectivity, operational data, Web capabilities, Source/Projection y scopes/aplicaciones.

`backend/` representa backend jobs y capacidades propias de esos jobs. La lógica Python server-side cuya responsabilidad es Web pertenece a `web/`. Connectivity es dual-use y no adquiere ownership funcional de sus consumidores.

## Storage Topology y Users durable

`web/capabilities/storage/topology` define contratos provider-neutral para resource topology.

Users declara un único recurso durable runtime confirmado:

```text
logical_id              users.runtime
owner                   users
provider                cosmos
default_physical_name   users-runtime
partition_key_path      /id
default_ttl_seconds     None
```

Invariantes CURRENT:

- Pending y Managed comparten `users.runtime`.
- `id == partition key == user_id`.
- `user_id = build_user_key(issuer, subject_id)`.
- No TTL automático para datos durables Users.
- Connection binding lo provee composición.
- No se crean `users.pending`, `users.managed`, `users.projection` ni `profiles.runtime` por defecto.
- `CosmosUsersRuntimeStore` implementa `UsersRuntimeStore` + `PendingUsersReader`.
- `observe()` es create-only + conflict reread; no usa blind upsert.
- Managed updates usan ETag/CAS.
- Managed removal conserva el documento como Resolved, disabled y `managed_state=retired`; re-add restaura `managed_state=present`.

## Source / Projection exact-release

Contratos congelados:

- release identity != content hash;
- dos releases pueden compartir content hash;
- Source current lo decide Source, nunca Cosmos;
- Projection target ejecutable = `SourceKey + SourceReleaseRef`;
- `project(target)` usa la release exacta y no relee current;
- `CURRENT / OUTDATED` compara identidad de release;
- retry conserva el mismo target;
- `SourceReleaseRef` transporta la identidad resoluble exacta;
- no introducir shim `SourceReleaseId <-> str`.

Una exact Source release de Users Configuration contiene dos resources contractualmente separados:

```text
users/configuration.json.gz
profiles/configuration.json.gz
```

La release sigue siendo única y atómica desde la perspectiva de Source. No existe Source independiente de Profiles ni segundo coordinator.

Escritura nueva:

- Users source document schema `2`;
- Profiles source resource schema `1`;
- `users/configuration.json.gz` contiene `UsersConfiguration` + `published_by`;
- `profiles/configuration.json.gz` contiene `ProfilesConfiguration`;
- Guest durable fields no forman parte del contrato canónico nuevo.

Lectura histórica durable:

- Source schema Users `1` continúa soportado;
- se normaliza `UsersConfigurationCatalog` histórico hacia contratos separados;
- Administrator se materializa como Profile explícito;
- campos Guest históricos no crean Profile funcional.

Projection canónica vigente:

```text
ProjectionRecord[UsersProfilesConfiguration]
```

El Cosmos canonical Projection store escribe schema `2`, puede leer schema `1`, conserva exact-release provenance y CAS/ETag.

El provenance legacy dentro de `users.runtime` todavía usa `projection_source_revision`; su migración exact-release sigue PLANNED.

## Profiles / Users canonical configuration

`ProfilesConfiguration` vive en Profiles y conserva Profiles funcionales explícitos.

Ownership CURRENT:

- Profiles no depende de Users.
- Users puede depender de Profiles.
- Profiles no depende de ADA.
- `ProfilesConfiguration` no implica Source/Projection independiente.
- No inferir `profiles.runtime`.

`ProfileCatalog` permanece semánticamente puro:

- catálogo vacío significa vacío;
- sólo contiene `ProfileDefinition` explícitos;
- no fabrica Local, Administrator ni Guest;
- duplicate normalized keys fallan;
- `require()` normaliza la key.

`UsersConfiguration` posee exclusivamente Managed Users.

`UsersProfilesConfiguration` posee la validación cross-contract:

- exige Profile `administrator`;
- prohíbe Profiles funcionales `guest` y `local`;
- cada Managed User, enabled o disabled, debe referenciar un Profile existente.

## Admin composition y browser workspace

El backend canónico opera directamente sobre `UsersProfilesConfiguration`.

Contratos CURRENT:

- `UsersProfilesAdminState`;
- `UsersProfilesAdminDraft`;
- `UsersProfilesAdministrationService`;
- operaciones puras de edición sobre `UsersProfilesConfiguration`.

`UsersProfilesAdminDraft` schema `2` conserva:

- `owner_subject_id`;
- `configuration`;
- exact `SourceSnapshot`;
- `revision`;
- `base_payload_revision`;
- `saved_at_utc`.

Invariantes locales:

- `revision` identifica el payload local actual;
- `base_payload_revision` identifica la BASE local;
- create nace clean;
- `has_local_changes` depende sólo de revisiones locales;
- edit preserva BASE + exact Source snapshot;
- rebase adopta exact Source snapshot nuevo;
- local revision no es Source identity;
- parser schema `2` no adapta schema `1`.

El Manager exact workspace usa el mismo principio:

- WORKSPACE local y SOURCE current son independientes;
- validación debe corresponder a la revisión local actual;
- verificación compara contra exact `SourceSnapshot`;
- publication exacta usa el snapshot verificado;
- publicación exitosa rebasa el workspace al snapshot publicado;
- no existe force publish para el camino exacto Users.

## Users exact Manager lifecycle

En el módulo `users` del ADA Configuration Manager:

```text
workflow_service               = None
draft_validation_service       = USERS_DRAFT_VALIDATION_SERVICE
exact_source_reader_service    = USERS_EXACT_SOURCE_READER_SERVICE
exact_source_history_service   = USERS_EXACT_SOURCE_HISTORY_SERVICE
exact_source_workflow_service  = USERS_EXACT_SOURCE_WORKFLOW_SERVICE
exact_projection_service       = USERS_EXACT_PROJECTION_SERVICE
```

Estado funcional CURRENT:

```text
validate        EXACT
read Source     EXACT
publish Source  EXACT
status          EXACT
project         EXACT
history list    EXACT
history read    EXACT
history preview EXACT
history -> work EXACT
legacy workflow NONE
```

`UsersManagerWorkflowAdapter` fue retirado del host ADA y de su API pública.

### Validation

`UsersManagerDraftValidationWorkflow`:

- calcula la revisión local mediante `build_workspace_revision`;
- parsea estrictamente `UsersProfilesConfiguration`;
- no adapta al catálogo legacy.

### Exact Source read/publication

`UsersManagerExactSourceReaderWorkflow` devuelve `ExactSourceReadResult`.

`UsersManagerExactSourceWorkflow`:

- expone `SourceSnapshot`;
- publica con `expected_source_snapshot`;
- parsea `UsersProfilesConfiguration`;
- obtiene actor desde provider explícito;
- conserva `PublishResult` tipado.

### Exact Projection

`ExactProjectionWorkflow` expone:

```text
get_status() -> projection.core.ProjectionStatus
get_current_projection_target() -> ProjectionTarget | None
project(target: ProjectionTarget) -> projection.core.ProjectionExecutionResult
```

`UsersManagerExactProjectionWorkflow` delega a `SourceProjectionService[UsersProfilesConfiguration]` y valida el `SourceKey`.

El status exacto se presenta sin convertirlo a `Manager ProjectionStatus` legacy y sin inventar:

- actor;
- projection revision;
- projection audit timestamp.

Browser state exacto usa nombres de release:

- `source_release_id`;
- `source_published_at_utc`;
- `projected_source_release_id`;
- `projected_source_published_at_utc`.

No usa `source_revision`.

### Exact History

Manager define:

```text
ExactSourceHistoryWorkflow
    list_history_exact(...) -> HistoryPage
    load_history_release_exact(SourceReleaseRef) -> ExactSourceHistoryReadResult
```

Users compone `UsersManagerExactSourceHistoryWorkflow` sobre `UsersProfilesAdministrationService`.

Invariantes:

- History durable contiene publicaciones Source reales, no autosaves;
- lista conserva `HistoryPage`;
- lectura exige `SourceReleaseRef` completo;
- Manager rechaza una respuesta cuya release difiera de la solicitada;
- no hay conversión a `RevisionHistoryEntry`;
- no hay `SourceReleaseId -> revision: str`;
- preview Users parsea `UsersProfilesConfiguration`.

### Historical release -> local work

Abrir una release histórica no repunta Source current.

Al cargar History como trabajo:

- si existe workspace, sólo se reemplaza el payload local;
- si no existe workspace, se crea primero una BASE desde Source current y luego se aplica el payload histórico;
- la BASE conserva el exact `SourceSnapshot` current;
- el workspace queda con cambios locales;
- para volver a Source debe ejecutarse validate → verify → publish;
- una publicación posterior crea una release nueva.

Esto preserva la regla Source: restore publica una nueva release; nunca repunta current directamente a una release histórica.

## Productive host boundary

El host ADA ya no registra un lifecycle legacy para Users.

Registra separadamente las capabilities exactas de Users y recibe una `ExactProjectionWorkflow` ya compuesta mediante `ConfigurationManagerDependencies.users_exact_projection`.

La composición ADA no extrae stores privados ni recompone la Projection.

`ConfigurationManagerDependencies` todavía conserva otras dependencias legacy porque Navigation/Tools/KPI no forman parte de este cutover.

## Web compositions

`users-manager` conecta Manager con Users Configuration sin invertir ownership.

CURRENT exports relevantes:

- `UsersManagerDraftValidationWorkflow`;
- `UsersManagerExactSourceReaderWorkflow`;
- `UsersManagerExactSourceWorkflow`;
- `UsersManagerExactSourceHistoryWorkflow`;
- `UsersManagerExactProjectionWorkflow`.

No usar `compositions/` como capa obligatoria ni como cajón general.

## Legacy administrative configuration

`UsersConfigurationCatalog`, `UsersAdministrationService`, `UsersConfigurationBundle` y contratos legacy pueden seguir presentes para consumidores no migrados.

Refinamiento CURRENT:

- ya no son contrato del editor Users activo;
- ya no son workflow Manager Users productivo;
- ya no son History preview Users activo;
- no adaptar el nuevo authoring canónico de vuelta a `UsersConfigurationCatalog`;
- compatibilidad durable/import histórica permanece explícitamente separada.

## Qualification del checkpoint `384a68fe...`

VERIFIED en el workspace reportado:

```text
Manager + Users Configuration + users-manager focused suite   238 passed
ADA full suite                                                56 passed / 4 failed
atlanticus:main remote                                        384a68fe... VERIFIED
```

Los cuatro failures ADA restantes fueron adjudicados fuera de Users:

- `KpiConfigurationManagerWorkflowAdapter`;
- `KpiDefinitionManagerWorkflowAdapter`;
- `ToolConfigurationManagerWorkflowAdapter`;
- `NavigationManagerWorkflowAdapter`.

Causa: adapters legacy todavía esperan/producen el contrato Projection anterior basado en revisiones textuales, mientras Manager vigente exige `ProjectionTarget` y `ProjectionExecutionResult.target`.

Por tanto:

```text
USERS EXACT MANAGER LIFECYCLE         CLOSED / VERIFIED / CURRENT
ADA FULL SUITE                        BLOCKED BY NON-USERS LEGACY PROJECTION ALIGNMENT
```

No se afirma:

- full Web suite completa ejecutada en `384a68fe...`;
- full ADA suite GREEN;
- Docker E2E;
- CI remoto;
- qualification de este checkpoint bajo Python 3.14.7.

## UNVERIFIED / OPEN relevantes

- constructor físico externo que suministre en ejecución real `users_profiles_administration` y `users_exact_projection`;
- Docker E2E del host completo;
- qualification visual browser productivo del History exacto si no existe evidencia separada;
- Python 3.14.7/Trixie global;
- runtime canonical cutover;
- exact-release provenance en `users.runtime`;
- Profile replacement UX;
- cleanup legacy global.

## Siguiente frontera recomendada

Un único foco:

```text
ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT
PLANNED / NEXT
```

Objetivo: alinear únicamente Navigation, Tools, KPI y KPI Definitions con el contrato Projection vigente de Manager.

No reabrir Users exact lifecycle para resolver esos fallos.

No mezclar:

- Users runtime provenance;
- Python migration;
- Root physical configuration;
- Navigation administrative migration general;
- legacy deletion global;
- Docker E2E general.
