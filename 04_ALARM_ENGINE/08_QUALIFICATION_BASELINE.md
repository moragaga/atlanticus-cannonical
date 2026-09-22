# Alarm Engine — Qualification Baseline

Estado: **R3.5 CLOSED PASS/GREEN / B.2 PURE RESOLVER IMPLEMENTATION EVIDENCE ADDED**

## Campaña R3.5

`alarm_test/` conserva una campaña acumulativa R3.5 con fases A, B, C, D, E y F, múltiples revisiones
del planner y contratos/checkpoints.

No usar solo el último XLSX para borrar la genealogía de findings.

## Hallazgos de método

La campaña aisló presiones: source unavailable, invalid candidate, lease loss, cache promotion failure,
drain y soak no se combinaron arbitrariamente. Esto evita adjudicar una falla a múltiples causas.

## E-008

Source Unavailable / CACHE_FALLBACK.

Propiedad: fallback controlado bajo source no disponible; no confundir ausencia de source con
autorización para adoptar estado inválido.

Estado de campaña: CLOSED según planner v1.0.92.

## E-009

Invalid Source Candidate.

Propiedad: candidate inválido no debe sustituir estado válido/adoptado.

Estado: CLOSED según planner v1.0.95.

## E-010

Lease Lost After WAL Before Cache.

Hubo run inicial `ABORTED / NOT ADJUDICATED`.

Antes de reintento:
- cumulative harness GREEN;
- 332/332 tests;
- identidades sintéticas canónicas;
- exact-second logical clock.

Estado: CLOSED según planner v1.0.103.

La prueba conecta directamente con fencing/recovery.

## E-011

Cache Promotion Failure.

Primer resultado aparente fue error de adjudicación del harness:
- durable prefix era correcto;
- snapshots reset vacíos omitían deliberadamente `state_basis`;
- el adjudicador esperaba algo que el contrato no exigía.

Corrección:
- comprobar `last_commit_id`;
- exigir ausencia de `state_basis` en empty reset snapshots.

Resultado: **harness finding, no product finding**.

Estado: CLOSED según planner v1.0.110.

## E-012

Drain Under Workload.

Geometría recuperada:
- 1000 alarmas;
- 10 priority groups;
- 600 s;
- iteration 5 s;
- refresh 10 s;
- 480 management inputs;
- 480 decisions;
- stop/drain alrededor de +300 s.

Hubo `Drain Cancellation Product Finding` y posterior `Product Fix Ready`.

Estado: CLOSED según planner v1.0.117.

## F-001

Soak 500 Local 30m.

- 500 alarmas;
- 1800 s;
- cadence 5 s;
- refresh 10 s;
- warmup 300 s;
- cinco ventanas estables de 300 s;
- esperado 361 iteraciones;
- CPU como caracterización; boundedness/cadence/integrity sí adjudican.

Estado: CLOSED planner v1.0.122.

## F-002

Soak 1000 Local 30m.

Misma geometría temporal de F-001, 1000 alarmas.

Reutiliza adjudicador temporal; no inventa nueva instrumentación.

Estado: CLOSED planner v1.0.126.

## F-007

Capacidad física/Docker/dataset bank.

Artefactos preservados:
- Docker constrained saturation;
- physical capacity search;
- real volume v2;
- dataset capture manifest;
- controlled physical dataset bank;
- templates de representativeness y synthetic conformance.

Estado: CLOSED / F010 proposed en planner v1.0.135.

## F-010 Final Docker Qualification

Cierre final recuperado:

- `CLOSED PASS/GREEN`;
- run `09311e68`;
- E2 = 1 CPU / 2 GiB;
- 1000 alarmas;
- 1800 s;
- 361/361 iteraciones;
- 0 overruns;
- p50 3357.821 ms;
- p95 3500.548 ms;
- p99 4290.209 ms;
- 2121 durable records;
- journal aligned;
- audit PASS;
- management requests 480/480;
- management decisions 480/480;
- compatible adoption 1000;
- threshold 0.50 -> 0.75;
- sin product findings abiertos;
- F011 profiling no requerido.

## Pure B.2 Resolver — qualification observada

Checkpoint implementado:

```text
moragaga/atlanticus:main
9398786ae9af7c00de1bcca9d7a311fe9ef2155f
```

El incremento cubre:
- READY Runtime + Delivery atómico;
- disabled Rule y evaluator qualification;
- Tool existence + GREEN;
- C1/C2/C3;
- C2 cumulative offsets y disabled-step exclusion;
- Process/Integrated Operations/Strategic visual target behavior;
- active/inactive Messages y deactivation override;
- deterministic findings;
- minutos -> segundos para reappearance.

Ejecución local reportada:

```text
uv run pytest
31 passed in 0.07s

uv run ruff check .
All checks passed!
```

El último `ruff format --check .` mostrado antes de la publicación del commit todavía indicaba:

```text
commented/.../resolver.py
src/.../resolver.py
```

como archivos a reformatear.

No existe en la evidencia de este cierre una salida posterior explícita del formatter.

Por tanto:

```text
B.2 behavior/unit tests   VERIFIED / GREEN
B.2 lint                  VERIFIED / GREEN
B.2 implementation main  VERIFIED / CURRENT
B.2 final format gate     UNVERIFIED
```

Confirmar una sola vez el formatter sobre el checkpoint CURRENT como gate de entrada del próximo
incremento. No repetir la qualification funcional si no aparece nueva evidencia.

## Regla posterior

El cierre R3.5 redirigió hacia productization/clean integration.

El cierre B.2 pure resolver redirige hacia el **Materialization Process**.

No repetir campañas largas ni reabrir resolver por rutina; hacerlo sólo ante nueva evidencia/riesgo.
