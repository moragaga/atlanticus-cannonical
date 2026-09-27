# Alarm Engine — Qualification Baseline

Estado: **R3.5 HISTORICAL CLOSED PASS/GREEN + B.2/ROUTING COMPONENT CHECKPOINT VERIFIED (2026-09-26) / EXECUTABLE MATERIALIZATION v0.2.1 REAL RERUN UNVERIFIED (2026-09-27)**

Este documento conserva la genealogía de qualification; **no convierte** tests unitarios recientes en una campaña E2E nueva. Las evidencias históricas y el nuevo proceso pertenecen a fronteras diferentes.

## Campaña histórica R3.5

`alarm_test/` conserva fases A, B, C, D, E y F y múltiples revisiones de planner. No usar sólo el último XLSX para borrar la genealogía de findings o adjudicar errores del harness al producto.

- **E-008, Source Unavailable / CACHE_FALLBACK:** fallback controlado no autoriza adoptar estado inválido. CLOSED según planner v1.0.92.
- **E-009, Invalid Source Candidate:** no sustituir estado válido/adoptado con candidato inválido. CLOSED según planner v1.0.95.
- **E-010, Lease Lost After WAL Before Cache:** primer run `ABORTED / NOT ADJUDICATED`; antes de reintento harness GREEN 332/332, identidades sintéticas canónicas y exact-second clock. CLOSED según planner v1.0.103.
- **E-011, Cache Promotion Failure:** aparente fallo procedía del adjudicador: snapshots vacíos tras reset omiten deliberadamente `state_basis`. Se corrigió adjudicación usando `last_commit_id` y ausencia de `state_basis`. **Finding del harness, no del producto**. CLOSED según planner v1.0.110.
- **E-012, Drain Under Workload:** 1000 alarmas, 10 priority groups, 600 s, cadence 5 s, refresh 10 s, 480 management requests y 480 decisions, stop/drain ~+300 s. Se identificó y corrigió Drain Cancellation Product Finding. CLOSED según planner v1.0.117.
- **F-001, Soak 500 Local 30m:** 500 alarmas, 1800 s, cadence 5 s, refresh 10 s, warmup 300 s, cinco ventanas estables de 300 s, objetivo 361 iteraciones. CPU como caracterización; boundedness/cadence/integrity sí adjudican. CLOSED planner v1.0.122.
- **F-002, Soak 1000 Local 30m:** misma geometría temporal, 1000 alarmas; reutiliza adjudicador. CLOSED planner v1.0.126.
- **F-007, Physical/Docker/dataset bank:** artefactos de saturación Docker, búsqueda física de capacidad, real volume v2, manifest de capture y templates representativeness/synthetic conformance. CLOSED / F010 proposed en planner v1.0.135.

## F-010 Final Docker Qualification — historical CLOSED PASS/GREEN

```text
run             09311e68
capacity        E2 = 1 CPU / 2 GiB
alarms          1000
duration        1800 s
iterations      361/361
overruns        0
p50             3357.821 ms
p95             3500.548 ms
p99             4290.209 ms
durable records 2121
journal aligned / audit PASS
management      480/480 requests, 480/480 decisions
adoption        compatible 1000
threshold       0.50 -> 0.75
```

Sin product findings abiertos en esa campaña. F011 profiling no requerido. La campaña no prueba el nuevo job de Materialization que todavía no existía en ese corte.

## Pure B.2 — checkpoint histórico

```text
moragaga/atlanticus@9398786ae9af7c00de1bcca9d7a311fe9ef2155f
```

El hito demostró resolver puro: READY Runtime/Delivery atómico, qualification evaluator y Tool, C1/C2/C3, offsets acumulados C2 y exclusión de steps disabled de ejecución, visual Process/Integrated Operations/Strategic, Messages/deactivation, findings deterministas y reappearance minutos->segundos. En ese corte se comunicaron 31 tests y lint GREEN; el formatter de resolver estaba pendiente de un rerun explícito. Esa limitación histórica se conserva, **no** se extrapola al estado CURRENT.

## Evidencia nueva acotada al 2026-09-26

Repositorio inspeccionado:

```text
atlanticus:main @ 411aea44ac60c09d2b07ce41d34c3f378788b97b
```

Tras los incrementos de strict routing backend y frontend, el usuario ejecutó localmente:

```text
domain/alarms:
  pytest -q                       GREEN / 56 puntos visibles
  ruff check .                    GREEN
  ruff format --check .           GREEN

backend/alarms/materialization:
  pytest -q                       GREEN / 49 puntos visibles
  ruff check .                    GREEN
  ruff format --check .           GREEN

web/alarms/configuration:
  pytest -q                       GREEN / 114 puntos visibles
  ruff check .                    GREEN
  ruff format --check .           GREEN
```

Las ejecuciones del usuario se produjeron después de cada parche y antes de su commit de cierre `411aea...`. Git confirma que ese HEAD contiene la policy, el resolver, los tests direccionales y el frontend correspondiente. **UNVERIFIED:** repetir esas suites sobre un checkout limpio del SHA final y host/browser E2E posterior al routing; tampoco se verificó Blob/Cosmos real, productores de qualification ni job/materialized stores.

## Evidencia nueva del proceso ejecutable v0.2.1 — 2026-09-27

HEAD inspeccionado por lectura Git: `moragaga/atlanticus@b600ca591b56d0924aed752dfae6e9fab2c6f1d6`. `backend/processes/alarms-materialization` existe como proceso ejecutable v0.2.1, pero aún escribe resultados en **Cosmos de salida**. El cambio arquitectónico acordado a volumen local todavía no fue implementado.

Prueba local del usuario **sobre v0.2.0, antes de corrección**:

```text
uv sync --python 3.14.2        PASS (48 paquetes instalados)
uv run pytest -q               11 FAILED por fixture 'tool-a' inválida; 19 PASS visibles
uv run ruff check .            5 I001 de imports
uv run ruff format --check .   10 archivos pendientes
uv run python -m compileall -q src  sin errores visibles
uv build --wheel               PASS / wheel v0.2.0
```

El valor `'tool-a'` no respeta el validador `^[a-z][a-z0-9_]*$` del paquete real Tools. En el HEAD v0.2.1 se observa fixture `tool_a` y ajustes de formato. **UNVERIFIED:** la suite real completa, Ruff y wheel **después** de esa corrección. Los 30 tests comunicados previamente usando un harness de dobles no son evidencia de integración sobre las dependencias reales.

No hay infraestructura Cosmos disponible para el usuario en este corte. No confundir ese bloqueo de E2E con imposibilidad de ejecutar tests locales del proceso.

## Gate del foco siguiente — limitado a Materialization local

No repetir campañas R3.5 completas sin finding. Antes de cambiar código, releer main 0.2.1 y utilidades existentes de publicación local; rerun pruebas reales y formateo. Después de acordar el contrato de volumen, verificar inputs exactos Rn/Cn, proveniencia, READY/BLOCKED, publicación atómica observable de Runtime+Delivery, integridad, idempotencia/retry, fallos antes de hacer visible READY y lectura local. La prueba contra Cosmos/Blob real se difiere hasta disponer de infraestructura. Runtime Adoption y Live Delivery son otros hitos.
