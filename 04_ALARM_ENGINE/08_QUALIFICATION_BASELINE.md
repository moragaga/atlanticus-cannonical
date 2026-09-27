# Alarm Engine — Qualification Baseline

Estado: **R3.5 HISTORICAL CLOSED PASS/GREEN / A Y B1 LOCAL UNIT GATES CLOSED / E2E NUEVA FRONTERA UNVERIFIED (2026-09-27)**

El historial R3.5 se conserva como evidencia **de su propio producto y corte**. Los tests recientes de Materialization/Runtime son pruebas unitarias y de contratos locales: **no** se convierten en una campaña de qualification E2E ni prueban Cosmos/Blob productivos.

## Campaña histórica R3.5 — sin reinterpretación

`atlanticus-decisions:main/alarm_test/` conserva fases A/B/C/D/E/F y sucesivas versiones del planner. No borrar su genealogía usando sólo el XLSX final ni adjudicar automáticamente problemas del harness al producto.

- **E-008, Source Unavailable / CACHE_FALLBACK:** fallback controlado no autoriza adoptar estado inválido. CLOSED, planner v1.0.92.
- **E-009, Invalid Source Candidate:** no sustituir estado válido/adoptado por candidato inválido. CLOSED, planner v1.0.95.
- **E-010, Lease Lost After WAL Before Cache:** primer run `ABORTED / NOT ADJUDICATED`; antes de reintento harness GREEN 332/332, identidades sintéticas canónicas y reloj exact-second. CLOSED, planner v1.0.103.
- **E-011, Cache Promotion Failure:** aparente fallo pertenecía al adjudicador; snapshots vacíos tras reset omiten deliberadamente `state_basis`. La adjudicación se corrigió usando `last_commit_id` y ausencia de `state_basis`. **Finding del harness, no del producto**. CLOSED, planner v1.0.110.
- **E-012, Drain Under Workload:** 1000 alarmas, 10 priority groups, 600 s, cadence 5 s, refresh 10 s, 480 management requests y 480 decisions, stop/drain ~+300 s; Drain Cancellation Product Finding identificado y corregido. CLOSED, planner v1.0.117.
- **F-001, Soak 500 Local 30m:** 500 alarmas, 1800 s, cadence 5 s, refresh 10 s, warmup 300 s, cinco ventanas estables de 300 s, objetivo 361 iteraciones; CPU como caracterización y boundedness/cadence/integrity como criterios. CLOSED, planner v1.0.122.
- **F-002, Soak 1000 Local 30m:** misma geometría temporal, 1000 alarmas. CLOSED, planner v1.0.126.
- **F-007, Physical/Docker/dataset bank:** saturación Docker, búsqueda de capacidad física, real volume v2, manifests de captura y templates de representativeness/synthetic conformance. CLOSED y F010 propuesto, planner v1.0.135.

### F-010 Final Docker Qualification — histórico CLOSED PASS/GREEN

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

Sin product findings abiertos en dicha campaña; F011 profiling no requerido. La campaña **no valida** el proceso nuevo de Materialization local, B1 ni un Effective Head global.

## Pure B.2 — checkpoint histórico

`atlanticus@9398786ae9af7c00de1bcca9d7a311fe9ef2155f`: resolver puro, READY Runtime/Delivery lógico coherente, qualification evaluadores y Tool, C1/C2/C3, offsets acumulados C2, steps disabled, validación visual y Messages/deactivation, findings deterministas, reappearance minutos a segundos. Se comunicaron 31 tests y lint GREEN; formatter del resolver sin rerun explícito en ese corte. No extrapolar esa limitación al estado actual ni confundir el paquete con el resolver: el paquete hoy también contiene lector I/O.

## Strict routing — checkpoint previo, 2026-09-26

Git: `atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b`. Tras incrementos de backend/frontend, el usuario informó localmente:

```text
domain/alarms:                  pytest 56 / ruff check PASS / format PASS
backend/alarms/materialization: pytest 49 / ruff check PASS / format PASS
web/alarms/configuration:      pytest 114 / ruff check PASS / format PASS
```

El código correspondiente está presente en ese checkpoint; repetir toda esa cadena en un checkout limpio y host/browser E2E no formó parte del gate. No equiparar estos tests con infraestructura Blob/Cosmos real.

## Materialization local + lector Runtime — Incremento A, 2026-09-27

La evidencia intermedia sobre v0.2.0/0.2.1 documentó fixtures inválidas `tool-a` y fallos de imports/format que se corrigieron en incrementos posteriores. No se mantienen como findings **abiertos** del corte actual; se conserva su historial en Git y en los checkpoints anteriores.

El usuario aplicó correcciones, ejecutó pruebas reales en su entorno y comunicó antes de integrar `atlanticus@9693e2b791b34624d551c52821274231ae05f2af`:

```text
uv sync --python 3.14.2 --all-packages                  PASS
backend/alarms/materialization:       pytest 49 PASS, Ruff PASS, format PASS, wheel PASS
backend/processes/alarms-materialization: pytest 43 PASS, Ruff PASS, format PASS, wheel PASS
backend/processes/alarms-runtime:      pytest 23 PASS, Ruff PASS, format PASS, wheel PASS
runtime/tests/test_local_configuration_reader.py: 7 PASS incluidos en 23
```

**CLOSED:** gate local del incremento A. Verificada presencia de código integrado en `9693e2b...`; no se adjudica qualification física multi-host ni integración operacional E2E.

## Artifact exacto y planificador Runtime — Incremento B1, 2026-09-27

El usuario aplicó el ZIP de B1 y la corrección Ruff del planificador, y compartió estos resultados locales:

```text
Workspace uv sync --python 3.14.2 --all-packages         PASS (56 paquetes resueltos)
backend/alarms/materialization:
    test_artifact_reference.py                          13 PASS
    pytest completo                                     62 PASS
    ruff check / ruff format --check                     PASS / PASS
    wheel                                               PASS, versión 1.0.0
backend/processes/alarms-runtime:
    test_adoption_plan.py                               17 PASS
    pytest completo                                     40 PASS
    ruff check / ruff format --check                     PASS / PASS tras fix
    wheel                                               PASS, versión 1.0.0
backend/processes/alarms-materialization:
    pytest completo                                     43 PASS
    ruff check / ruff format --check                     PASS / PASS
```

El commit `atlanticus@c8f23d91ae1cb817be55b4b812b22ffca518880e` incluye B1 y su corrección de formato. **CLOSED — gate local**, no equivalencia con una ejecución CI reproducida en checkout limpio de ese SHA. No se adjuntó una repetición nueva del wheel de `alarms-materialization` posterior al parche B1, que no modificó ese paquete operativo; su wheel PASS corresponde al incremento A.

## OPEN / límites de evidencia

- **UNVERIFIED:** ejecutables/evaluadores GREEN reales y qualification productiva, Manager -> Cosmos -> Materialization en infraestructura real, Blob/Cosmos E2E.
- **UNVERIFIED:** semántica de publicación y acceso al volumen definitivo en varios hosts/FS, pruebas de takeover físico reales fuera de los dobles locales.
- **PLANNED:** ejecución segura de `ADDED`/`ENABLED`, adopción durable y recovery de Effective Head; tests actuales validan **planificación**, no adopción global.
- **SEPARATE:** Delivery local exacto, Live, Management Capture y validaciones Web del nuevo ciclo.

Siguiente gate técnico: definir primero el contrato de Runtime Adoption durable sobre los componentes actuales, sin repetir campañas R3.5 por defecto ni usar fixtures sintéticas como evidencia de productores reales.
