# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Regla

Qualification y tests son evidencia de propiedades, no decoración de cobertura.

No reinterpretar un `FAIL` histórico como fallo vigente sin revisar si fue:

- finding de producto;
- defecto de harness;
- error de adjudicación;
- problema de reloj/routing/test;
- ejecución abortada/no adjudicada.

## Web Storage Topology / Users Storage Topology

Checkpoint de implementación:

`moragaga/atlanticus@d63886d8d42688e3d03d680f0a3d7b92cd3863ed`

Evidencia ejecutada en el workspace Web real para el cierre original de Topology:

- `WEB-STORAGE-TOPOLOGY`: 19 tests focalizados PASS/GREEN.
- Storage Topology + Users Storage: 39 tests focalizados PASS/GREEN.
- suite Web global: PASS/GREEN con 7 skips conocidos.
- Ruff check del alcance: PASS/GREEN.
- Ruff format check del alcance: PASS/GREEN.
- `uv lock`: PASS/GREEN.
- import público de Storage Topology: PASS/GREEN.
- `git diff --check`: PASS/GREEN.

Propiedades validadas:
- resolver inmutable y determinista;
- dedupe de declaraciones idénticas;
- rechazo de declaraciones incompatibles;
- rechazo de overrides desconocidos/prohibidos;
- connection binding obligatorio antes del provider;
- rechazo de colisión física;
- `CosmosContainerTopology` inmutable y con validación de partition key/TTL;
- `users.runtime` único, durable, partition `/id`, TTL `None`;
- Users permite override de conexión y prohíbe override de physical name;
- espejo comentado equivalente al código productivo en el nuevo alcance.

El cambio accidental de formato detectado fuera del alcance en `users/store.py` fue restaurado antes del cierre y no forma parte del checkpoint.

## Storage Preflight Cosmos Bridge

Estado:

```text
STORAGE-PREFLIGHT-COSMOS-BRIDGE  CLOSED / VERIFIED / CURRENT
```

Qualification ejecutada en workspace Web real:
- package bridge: 17 passed;
- Storage Topology + bridge: 51 passed;
- Storage Topology + bridge + Users core: 87 passed;
- suite Web: 448 passed, 7 skipped;
- `uv lock` / `uv sync`: PASS/GREEN;
- Ruff global: PASS/GREEN;
- Ruff format focalizado: PASS/GREEN;
- API público: PASS/GREEN;
- `git diff --check`: PASS/GREEN.

Propiedades demostradas:
- traducción determinista de `ResolvedStoragePlan` a `CosmosContainerSpec`;
- múltiples conexiones Cosmos por `connection_ref`;
- topology/spec/binding validation antes de provider I/O;
- provider no Cosmos ignorado;
- zero Cosmos resources = no-op;
- bridge sin Azure SDK, raw settings ni client construction;
- bridge no crea database.

## Users Cosmos Runtime Adapter

Estado:

```text
COSMOS-USERS-RUNTIME-ADAPTER  CLOSED / VERIFIED / CURRENT
```

Qualification final ejecutada en workspace Web real:
- package `capabilities/users/cosmos`: 23 passed;
- Storage Topology + Storage Cosmos + Users core + Users Cosmos: 110 passed;
- suite Web: 471 passed, 7 skipped;
- Ruff check: PASS/GREEN;
- Ruff format: PASS/GREEN;
- `uv lock` / `uv sync`: PASS/GREEN;
- contrato público `CosmosUsersRuntimeStore` implementando `UsersRuntimeStore` y `PendingUsersReader`: PASS/GREEN;
- `git diff --check`: PASS/GREEN.

Propiedades demostradas:
- `resolve()` point-read por `user_id`;
- `observe()` create-only, nunca upsert;
- conflicto concurrente reread del estado durable vigente;
- promoción/deshabilitación concurrente no es sobrescrita por Pending;
- `list_pending()` cross-partition con orden determinista;
- documento inválido/identity mismatch falla explícitamente;
- errores Cosmos se sanitizan hacia el contrato Users conservando causa;
- no provisioning ni Azure SDK dentro del adapter;
- mirror comentado equivalente.

## Users Runtime Projection Boundary

Checkpoint de implementación:

`moragaga/atlanticus@4758d993296bfe2a629a9aa3b8e4b486cf7b2305`

Estado:

```text
USERS-RUNTIME-PROJECTION-BOUNDARY  CLOSED / VERIFIED / CURRENT
```

Qualification final ejecutada en workspace Web real:
- tests focalizados Users core + Configuration runtime projection + Projection Cosmos: 33 passed;
- suite Web: 496 passed, 7 skipped;
- Ruff check del delta: PASS/GREEN;
- Ruff format check del delta: PASS/GREEN;
- `uv lock`: PASS/GREEN;
- `git diff --check`: PASS/GREEN.

Propiedades demostradas:
- `UsersRuntimeProjectionWriter` es contrato snapshot-level separado de `UsersRuntimeStore` y `PendingUsersReader`;
- `UsersRuntimeMaterializingProjectionRepository` materializa `users.runtime` antes de avanzar el catálogo/estado de proyección legacy;
- `CosmosUsersRuntimeProjectionWriter` vive en package provider-specific separado;
- Pending→Resolved conserva `id` y partition key;
- Managed removido se conserva como Resolved deshabilitado con `managed_state=retired`;
- re-add restaura `managed_state=present` y valores de configuración actuales;
- ausente pero configurado se crea directamente Resolved;
- writes existentes usan ETag/CAS y no blind upsert;
- conflicto concurrente con `observe()` se reconcilia sin sobrescribir silenciosamente;
- replay del mismo snapshot converge semánticamente;
- fallo parcial no avanza el estado global de proyección legacy;
- `UsersAccessResolver` rechaza Managed deshabilitado antes de requerir un perfil histórico retirado;
- mirror comentado equivalente dentro del alcance.

El provenance de este checkpoint sigue basado en `UsersConfigurationBundle.revision`, que es digest de contenido. No equivale a `SourceReleaseId`.

## Users Canonical Source

Checkpoint de implementación:

`moragaga/atlanticus@f996905c353de26c42bc4907e32a1f2f0c161648`

Estado:

```text
USERS-CANONICAL-SOURCE-1  CLOSED / VERIFIED / CURRENT
```

Qualification final ejecutada en workspace Web real:
- tests dirigidos del Source canónico de Users: 7 passed;
- suite completa `capabilities/users/configuration/tests`: 54 passed;
- suite Web: 503 passed, 7 skipped;
- Ruff check del delta: PASS/GREEN;
- Ruff format check del delta: PASS/GREEN;
- `uv lock`: PASS/GREEN;
- `git diff --check`: PASS/GREEN.

Propiedades demostradas:
- `UsersSourceCodec` serializa un único recurso canónico `users/configuration.json.gz`;
- JSON compacto + gzip determinista para el recurso de dominio;
- `UsersSourcePayload` conserva `UsersConfigurationCatalog + published_by`;
- `UsersSourceRelease` combina payload de dominio con `SourceReleaseMetadata`;
- `UsersSourceService` usa `SourceStore` y no reimplementa release identity, History ni CAS;
- publicación usa `ConcurrencyToken` y `basis_release`;
- lectura de current selecciona una release y luego hidrata esa release exacta;
- mismo contenido puede republicarse como otra release con identidad distinta;
- metadata devuelta con `SourceKey` o `SourceReleaseRef` inconsistente falla explícitamente.

## Users Canonical Projection

Checkpoint de implementación:

`moragaga/atlanticus@139ee93a118e51f66c3d585f00235f212a2475c1`

Estado:

```text
USERS-CANONICAL-PROJECTION-2  CLOSED / VERIFIED / CURRENT
```

Packages actuales:

```text
atlanticus-web-users-configuration==0.1.9
atlanticus-web-users-projection-cosmos==0.1.1
```

Qualification final ejecutada en workspace Web real:
- tests nuevos focalizados Source→Projection + Cosmos store: 10 passed;
- `capabilities/users/configuration/tests` + `capabilities/users/projection-cosmos/tests`: 84 passed;
- suite Web global: 513 passed, 7 skipped;
- `uv lock --check`: PASS/GREEN;
- Ruff check del alcance: PASS/GREEN;
- `git diff --check`: PASS/GREEN.

Ruff format:
- `services.py`, modificado por el incremento, fue reformateado;
- el check focalizado posterior no reportó ningún archivo modificado por este hito;
- permanecieron reportados tres archivos Web preexistentes no modificados: `web/callbacks.py`, `web/ids.py`, `web/layout.py`;
- esos archivos quedan fuera del alcance de esta qualification.

Propiedades demostradas:
- `UsersProjectionBuilder` construye `UsersConfigurationCatalog` desde recursos de una release Source exacta;
- `SourceProjectionService` ejecuta el target exacto sin reread de current durante la proyección;
- una release histórica seleccionada puede proyectarse y luego quedar `OUTDATED`;
- mismo contenido en dos Source releases distintas produce dos targets distintos;
- `CosmosUsersConfigurationProjectionStore` persiste provenance canónico exact-release;
- first write create-only;
- replace con ETag/CAS;
- no blind upsert;
- replay same-target + same payload es idempotente y preserva el active existente;
- same release ID con metadata o payload incompatible falla como invariante;
- conflicto concurrente same-target converge;
- conflicto different-target produce `UsersConfigurationProjectionConflictError`;
- target histórico explícito puede activarse secuencialmente;
- no se infiere ordering por release ID ni timestamp.

No demostrado por este hito:
- provisioning físico del container del canonical Projection store;
- materialización exact-release de `users.runtime`;
- migración Manager/Users administrativa;
- eliminación de contracts legacy.

## Manager Root Canonical Cutover

Checkpoint de implementación:

`moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06`

Estado:

```text
MANAGER-ROOT-CANONICAL-CUTOVER  CLOSED / VERIFIED / CURRENT
```

Qualification ejecutada en el workspace Web real antes de integrar el commit:
- `uv lock --check`: PASS/GREEN;
- `uv run ruff check capabilities/manager`: PASS/GREEN;
- `uv run pytest capabilities/manager/tests -q`: 76 passed;
- `uv run pytest -q`: 514 passed, 7 skipped;
- `git diff --check`: PASS/GREEN;
- el diff del hito modificó exactamente 9 archivos del Manager, incluidos mirrors comentados y tests.

Propiedades demostradas:
- `ConfigurationLifecycleWorkflow.get_current_projection_target()` expone un target canónico exacto;
- `ConfigurationLifecycleWorkflow.project(...)` recibe `ProjectionTarget` y no una revisión textual;
- `ProjectionExecutionResult.target` conserva la identidad exacta ejecutada;
- `ManagerProjectionCoordinator.project(...)` transporta ese target sin convertirlo a string ni volver a seleccionar current;
- el callback productivo selecciona current server-side inmediatamente antes de ejecutar;
- browser state no suministra la identidad ejecutable de Project;
- el signal de Projection contiene `source_key`, `source_release_id`, `source_published_at_utc` y `projection_revision`;
- el botón Project depende de que exista un target exacto;
- un target histórico explícito es transportado intacto y no reemplazado por current;
- no se introdujo shim `SourceReleaseId <-> str` ni un segundo coordinator paralelo.

No demostrado ni cerrado por este hito:
- migración de los contratos administrativos de publicación/verificación/history que siguen usando revisiones textuales;
- migración administrativa de Users o Navigation;
- provenance exact-release dentro de `users.runtime`;
- IndexedDB para WORKSPACE;
- eliminación de contracts/adapters legacy de dominio;
- resource topology del canonical Users Projection store;
- orchestration multi-capability;
- CI remoto para el commit: GitHub no expone status checks asociados al checkpoint.

La formulación previa “Manager productive coordinator cutover” queda refinada: este cierre corresponde específicamente al root productivo de la acción Projection.

## Profiles Domain Extraction

Checkpoint de implementación:

`moragaga/atlanticus@b34581958e8d59f2cd14e47f56c4309ee76027fb`

Estado:

```text
PROFILES-DOMAIN-EXTRACTION  CLOSED / VERIFIED / CURRENT
```

Qualification ejecutada en el workspace Web real:
- `uv lock`: PASS/GREEN;
- `uv sync`: PASS/GREEN;
- tests focalizados Profiles + Users core + Users Configuration: 92 passed;
- suite Web global: 512 passed, 7 skipped;
- `uv run ruff check . --fix`: 7 imports ordenados automáticamente, 0 restantes;
- `uv run ruff check .`: PASS/GREEN;
- búsqueda del import eliminado `from atlanticus.web.users.profiles import`: 0 coincidencias Python fuera de `.venv`.

Propiedades demostradas:
- existe `web/capabilities/profiles/core` como capability y workspace package propio;
- package `atlanticus-web-profiles==0.1.0` está incorporado al workspace/lock;
- `ProfileDefinition`, `ProfileCatalog`, constantes y normalizadores pertenecen al namespace `atlanticus.web.profiles`;
- Profiles posee `ProfilesDefinitionError` y no importa Users;
- Users core declara dependencia one-way en `atlanticus-web-profiles`;
- Users Configuration declara dependencia directa en `atlanticus-web-profiles`;
- consumidores productivos, tests y mirrors comentados fueron migrados al nuevo namespace;
- el módulo productivo `atlanticus.web.users.profiles` fue eliminado;
- no se introdujo re-export, alias ni shim del namespace viejo;
- los errores de definición de Profiles dejaron de depender de `UsersDefinitionError`;
- la semántica vigente de `ProfileCatalog` fue preservada durante la extracción para no mezclar ownership con rediseño semántico.

No demostrado ni cerrado por este hito:
- semántica final de `root`, `guest`, `local`, John/Jane o `administrator`;
- bootstrap identity contract para `root`;
- separación de `UsersConfigurationCatalog` en contratos Users y Profiles;
- Source/Projection independiente de Profiles;
- validación cross-domain final `user.profile_key` contra Profiles proyectados;
- rename o retiro de `PROFILE_CATALOG_SERVICE_KEY = 'atlanticus.web.users.profiles'`;
- reconciliación del DOCX histórico de Users/Profiles/Access;
- runtime exact-release provenance.

## Alarm Engine

La campaña R3.5 llegó a cierre final `PASS/GREEN`.

Baseline final recuperado:

- F-010: CLOSED PASS/GREEN.
- Run: `09311e68`.
- Envelope recomendado: E2 = 1 CPU / 2 GiB.
- 1000 alarmas.
- 1800 s.
- 361/361 iteraciones.
- 0 overruns.
- p50: 3357.821 ms.
- p95: 3500.548 ms.
- p99: 4290.209 ms.
- journal/durability audit PASS.
- 2121 registros durables.
- management requests 480/480.
- management decisions 480/480.
- sin finding de producto abierto al cierre.
- F011 profiling no requerido.

Detalle en `04_ALARM_ENGINE/08_QUALIFICATION_BASELINE.md`.

## Python/Trixie

Existe evidencia de construcción/prueba con:

`python:3.14.7-slim-trixie`

pero la migración global del repo no está materializada aún.

Los packages Users y Profiles afectados por los hitos previos aún declaran `requires-python ==3.14.2`; esta discrepancia es preexistente y no se resolvió dentro de Source/Projection, Manager root cutover ni Profiles Domain Extraction.

## ADA Web

Checkpoint histórico 31-08-2026 preserva GREEN para:
- session auto lifecycle;
- wake pulse;
- PWA surface;
- page readiness;
- wake lock;
- activity;
- card display;
- responsive header.

`RESPONSIVE-TIME-001` no se promueve a GREEN sin closure posterior.

La política vigente no trata validadores CSS visuales como qualification contractual.

Detalle en `13_ADA_WEB/`.
