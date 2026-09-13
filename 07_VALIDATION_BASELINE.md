# Atlanticus — Validation Baseline

Estado: **CANDIDATE**

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

El provenance de este checkpoint sigue basado en `UsersConfigurationBundle.revision`, que es digest de contenido. No equivale a `SourceReleaseId`; esa convergencia pertenece a `USERS-CANONICAL-PROJECTION-2`.

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
- metadata devuelta con `SourceKey` o `SourceReleaseRef` inconsistente falla explícitamente;
- `atlanticus-web-users-configuration==0.1.8` depende de `atlanticus-web-source==0.1.0`;
- los contratos legacy administrativos permanecen disponibles porque el consumer Manager productivo aún no ha migrado.

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
