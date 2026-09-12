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
