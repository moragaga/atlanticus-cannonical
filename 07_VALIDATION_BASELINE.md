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

`moragaga/atlanticus@d7b5e53b30012b0043f17c8a1e98c0ce03325bb2`

Evidencia ejecutada en el workspace Web real:

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
