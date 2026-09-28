# Alarm Engine — Qualification Baseline

Estado: **CURRENT — histórico R3.5 conservado y gates locales hasta B2c.7d; CI/distribución/Docker actual UNVERIFIED**. Corte 2026-09-28. No confundir logs locales aportados por el usuario con ejecución remota independiente ni validar un hito posterior usando benchmarks de otro árbol.

## 1. Campaña histórica R3.5 — CLOSED HISTÓRICA

Los documentos originales están en `atlanticus-decisions:main/alarm_test/`. Genealogía: E-008 Source Unavailable/CACHE_FALLBACK; E-009 Invalid Source Candidate; E-010 Lease Lost After WAL Before Cache (primera corrida abortada y cierre posterior); E-011 Cache Promotion Failure (finding del adjudicador/harness, no defecto de producto demostrado); E-012 Drain Under Workload; F-001 Soak 500 Local 30m; F-002 Soak 1000 Local 30m; F-007 Physical/Docker/dataset bank; F-010 Final Docker Qualification.

**F-010** cerró PASS/GREEN en su generación histórica: run `09311e68`, E2 = 1 CPU/2 GiB, 1000 alarmas, 1800 s, 361/361 iterations, 0 overruns, p50 3357.821 ms, p95 3500.548 ms, p99 4290.209 ms, 2121 durable records, journal audit/alignment PASS, 480/480 management requests y 480/480 decisions, adopción compatible 1000, threshold 0.50→0.75. **No extrapolar F-010 a los ejecutables nuevos B2c.7 ni a FACTS v2**.

## 2. Gates anteriores — contexto preservado

| Hito | Evidencia local conocida |
|---|---|
| Resolver B.2 inicial | 31 pruebas, lint GREEN del corte correspondiente. |
| Strict routing | Domain 56; Materialization 49; Web Configuration 114; Ruff/format PASS. |
| A, publicación READY/lector | Materialization 49; proceso 43; Runtime 23 PASS; gates locales pertinentes. |
| B1 referencia exacta | Materialization 62; Runtime 40; proceso 43 PASS. |
| B2a.1 WAL adopción V1 | Persistence 57 PASS, Ruff/format/diff PASS. |
| B2a.2 WAL V2 | 18 específicas PASS; Persistence y Ruff/format/diff PASS tras reparar fixture temporal. |
| B2b.1 Effective Head | 19 específicas y Persistence 94 PASS; Ruff/format/build locales. |
| B2b.2 lector exacto | 15 específicas y Runtime 55 PASS; Ruff/format/build locales. |
| B2c.5c fuentes/requisitos | 119 Runtime+integration y 436 regresión PASS; Ruff PASS, 33 archivos formateados, diff PASS. |
| B2c.5d catálogo y ejemplo aislado | 7 específicas y 443 regresión PASS después del traslado, Ruff PASS, 40 archivos formateados. |

Los SHAs históricos constan en `11_SOURCE_LEDGER.md` y las decisiones/harness originales. No asumir que una prueba histórica vuelve a pasar por el hecho de incorporar un módulo nuevo.

## 3. B2c.7 — resultados observados en este chat

| Corte | Evidencia aportada por usuario | Qué prueba y qué no |
|---|---|---|
| **B2c.7a** Engine CURRENT+FACTS inicial | `137 passed, 1 skipped`; Ruff check PASS; Ruff format 53 archivos PASS tras corrección; diff check PASS. Commit local `efe231d61c9d5a6f4eca1e3f22a201a9b3c1861b`. | Publicación bajo fixtures/ciclos y contrato local; no Docker ni Delivery externo. |
| **B2c.7b** Delivery input receiver | `151 passed, 1 skipped` (Engine+Delivery); Ruff check PASS; Ruff format 60 archivos PASS; diff check PASS. Commit local `94f26213ca28b550baf53d8ee34e34da7538ad17`, además leído en HEAD remoto. | Separación de consumidores, cursores e integridad de recepciones controladas. |
| **B2c.7c** integración Engine→Delivery | Prueba de integración 1 PASS; suite conjunta `152 passed, 1 skipped`; Ruff check PASS; 61 archivos formateados PASS. Commit local `199fc0f4ff543fef6ff204a892370a53bf905423`. | Engine real + datos NOTPII controlados + EFFECTIVE/READY + volumen compartido + recreación de componentes; **no** son dos contenedores independientes. |
| **B2c.7d** cadena FACTS v2 | Pruebas específicas `32 passed`; suite conjunta `162 passed, 1 skipped`; Ruff check PASS; 61 archivos formateados PASS. Commit declarado por el usuario y posteriormente verificado en Git `c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`. | Continuidad, corrupción, huecos/reordenación y recovery según tests; **no** migra volúmenes v1. |

No afirmar que se reejecutó la suite *después* de checkout limpio de `c67fcb5...`. El usuario ejecutó suites y posteriormente informó el commit; su árbol y comandos son evidencia local. Los artefactos específicos de B2c.7d y su suite usan `schema_version=2`. La prueba `1 skipped` sigue sin identificar en este cierre: consultar `pytest -rs` durante el próximo gate, sin asumir su causa.

## 4. Qualification pendiente — no confundir con fallo productivo

- **PLANNED / siguiente foco único:** build/distribución de ambos jobs, contenido real de wheels/distribución de esquemas, arranque independiente en Docker, volumen/credenciales correctos y reinicios de procesos. Revisar que la dependencia exacta `atlanticus-state==1.0.0` esté correctamente incluida en artefactos Engine.
- **UNVERIFIED:** CI sobre checkout limpio del último SHA; test saltado; Cosmos/Blob, datos PI reales, volumen multi-host, carreras de filesystem/lease, publicación externa y métricas bajo carga de esta generación.
- **BLOCKED si hay datos v1:** despliegue sobre volumen FACTS v1 sin inventario/migración controlada. No crear un adaptador legacy ni borrar historial para forzar PASS.
- **SEPARATE:** qualification operacional Tool/evaluators, Web/Management y Analytics. La suite B2c.7 no los certifica.

Cerrar el gate local B2c.7d **no** cierra el gate de distribución ni el riesgo de migración física.
