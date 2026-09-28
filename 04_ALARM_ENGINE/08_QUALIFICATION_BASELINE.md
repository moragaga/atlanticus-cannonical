# Alarm Engine — Qualification Baseline

Estado: **HISTORICAL R3.5 CLOSED PASS/GREEN / A, B1 Y B2a/B2b LOCAL GATES CLOSED / NUEVO E2E UNVERIFIED (2026-09-27)**.

Esta es una actualización documental de checkpoints, **no una nueva qualification**. Los tests recientes son locales, de contrato/recovery y construcción; no sustituyen campaña física Docker, CI limpia ni prueba E2E Cosmos/Blob/volumen multi-host. Autoridades verificadas: `atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`, `atlanticus-cannonical@be2c424c44648e6488daae36d410cf425eed02b8`, `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.

## Campaña histórica R3.5 — sin reinterpretación

`atlanticus-decisions:main/alarm_test/` conserva fases A/B/C/D/E/F y genealogía de planners. No borrar ni sustituir evidencia histórica mediante los resultados de los nuevos unit tests:

- **E-008, Source Unavailable / CACHE_FALLBACK:** fallback controlado no autoriza adoptar estado inválido. CLOSED, planner v1.0.92.
- **E-009, Invalid Source Candidate:** candidato inválido no reemplaza estado válido/adoptado. CLOSED, planner v1.0.95.
- **E-010, Lease Lost After WAL Before Cache:** primer run `ABORTED / NOT ADJUDICATED`; tras reparación del harness GREEN 332/332, identidades sintéticas canónicas y reloj exact-second. CLOSED, planner v1.0.103.
- **E-011, Cache Promotion Failure:** aparente fallo pertenecía al adjudicador; snapshots vacíos tras reset omiten `state_basis` deliberadamente. Adjudicación corregida mediante `last_commit_id` y ausencia de `state_basis`. Finding del **harness**, no del producto. CLOSED, planner v1.0.110.
- **E-012, Drain Under Workload:** 1000 alarmas, 10 priority groups, 600 s, cadence 5 s, refresh 10 s, 480 management requests y 480 decisions, stop/drain ~+300 s. Drain Cancellation Product Finding identificado/corregido. CLOSED, planner v1.0.117.
- **F-001, Soak 500 Local 30m:** 500 alarmas, 1800 s, cadence 5 s, refresh 10 s, warmup 300 s, cinco ventanas estables de 300 s, objetivo 361 iteraciones; CPU caracterización y boundedness/cadence/integrity como criterios. CLOSED, planner v1.0.122.
- **F-002, Soak 1000 Local 30m:** misma geometría, 1000 alarmas. CLOSED, planner v1.0.126.
- **F-007, Physical/Docker/dataset bank:** saturación Docker, capacidad física, real volume v2, manifests de captura, templates representativeness/synthetic conformance. CLOSED; F-010 propuesto, planner v1.0.135.

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

Sin product findings abiertos para esa campaña; F011 profiling no requerido. **Esta campaña no valida B2a/B2b ni la nueva salida local de Materialization.**

## Pure B.2 — checkpoint histórico

`atlanticus@9398786ae9af7c00de1bcca9d7a311fe9ef2155f`: resolver puro READY Runtime/Delivery, qualification evaluador/Tool, C1/C2/C3, offsets acumulados C2, steps disabled, validación visual y Messages/deactivation, findings deterministas y reappearance minutos a segundos. En su momento se informaron 31 tests y lint GREEN; formatter sin rerun explícito de ese checkpoint. El paquete actual integra también lector I/O, pero el resolver conserva pureza.

## Strict routing — checkpoint previo 2026-09-26

`atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b`: backend/frontend. Logs locales del usuario de ese checkpoint:

```text
domain/alarms:                  pytest 56 / ruff check PASS / format PASS
backend/alarms/materialization: pytest 49 / ruff check PASS / format PASS
web/alarms/configuration:      pytest 114 / ruff check PASS / format PASS
```

La presencia de código en Git se verificó; no representa E2E de host/browser ni Cosmos/Blob reales.

## Materialization local + lector Runtime — Incremento A

La evidencia intermedia sobre v0.2.0/0.2.1 contenía fixtures inválidas `tool-a`, fallos de import/formato corregidos después. Se conserva historial en Git; no reabrir esos findings como actuales. Logs locales del usuario antes de `atlanticus@9693e2b791b34624d551c52821274231ae05f2af`:

```text
uv sync --python 3.14.2 --all-packages                   PASS
backend/alarms/materialization:        pytest 49 PASS, Ruff/format/wheel PASS
backend/processes/alarms-materialization: pytest 43 PASS, Ruff/format/wheel PASS
backend/processes/alarms-runtime:       pytest 23 PASS, Ruff/format/wheel PASS
runtime/tests/test_local_configuration_reader.py: 7 PASS incluidos en 23
```

**CLOSED — gate local A**. E2E real y volumen multi-host no adjudicados.

## Artifact exacto y planificador Runtime — Incremento B1

Logs locales del usuario previos a `atlanticus@c8f23d91ae1cb817be55b4b812b22ffca518880e`:

```text
uv sync --python 3.14.2 --all-packages             PASS (56 paquetes)
backend/alarms/materialization:
   test_artifact_reference.py                     13 PASS
   suite                                          62 PASS
   Ruff/format/wheel                              PASS
backend/processes/alarms-runtime:
   test_adoption_plan.py                          17 PASS
   suite                                          40 PASS
   Ruff/format/wheel                              PASS tras corrección SIM102
backend/processes/alarms-materialization:
   suite                                          43 PASS
   Ruff/format                                    PASS
```

**CLOSED — gate local B1**. El wheel de `alarms-materialization process` citado históricamente corresponde al incremento A, no inventar un rerun posterior a B1.

## B2a.1 — adopción global durable sin grupos (2026-09-27)

Checkpoint exacto con archivos B2a.1: `atlanticus@3c616dab38a80467359c48c05144389db0211b80`. Se informó validación local final: suite Persistence 57 PASS; Ruff check y format PASS, `git diff --check` PASS. Una construcción wheel PASS se informó **antes del último formateo**; su repetición posterior no fue evidenciada. Código y tests presentes en el checkpoint. **CLOSED local**, no CI/E2E.

## B2a.2 — adopción V1/V2 con grupos (2026-09-27)

Commit de archivos específicos B2a.2: `atlanticus@e0578d3338138693430249803b6397e32f422227`. Inicialmente falló un test por fixture cronológica (`effective_at` posterior a `committed_at`), corregido **sólo en la prueba**. El usuario repitió 18 tests específicos PASS, suite Persistence PASS, Ruff check/format PASS, `git diff --check` PASS. Construcción de wheel/sdist informada PASS en log previo; no reejecutada tras corrección exclusiva de test. **CLOSED local**.

## B2b.1 — proyección EFFECTIVE (2026-09-27)

Commit específico del paquete Persistence: `atlanticus@3ce75d87f7158f2cd70b53e6a99864b4c42bede9`, contenido también en checkpoint `963c21d340be6bd157550d516ad32d97668fbc57`. Logs del usuario: 19 tests específicos `test_effective_head.py` PASS; suite Persistence **94 PASS**; Ruff check y format PASS; `git diff --check` PASS; sdist y wheel PASS, paquete `1.0.0`. La advertencia README de sdist no detuvo el build y no habilita añadir README en este cierre. **CLOSED local**.

## B2b.2 — selección de EFFECTIVE exacto en Runtime (2026-09-27)

Commit específico: `atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`. Primera tanda de comandos se ejecutó fuera del workspace y no encontró Ruff/pytest/pyproject; **no** fue un fallo de tests de producto. Desde `scopes/ada-command-center/backend`, logs del usuario: 15 tests específicos `test_effective_local_configuration.py` PASS; suite Runtime **55 PASS**; Ruff check/format PASS; `git diff --check` PASS; sdist/wheel `1.0.0` PASS. **CLOSED local**.

## OPEN — límites de evidencia

- **UNVERIFIED:** gates reproducidos de cero en checkout limpio del SHA final/CI; integración E2E Runtime Adoption global; condición operativa de volume multi-host/fencing real.
- **UNVERIFIED:** productores GREEN reales, Manager/Blob/Cosmos real, integraciones de entrada y salida; no confundir qualification JSON controlada con producer operacional.
- **PLANNED B2c:** completitud del ejecutor del plan B1 e integración con V1/V2/EFFECTIVE. La validación del lector B2b.2 no prueba que el job actual adopte automáticamente esa configuración.
- **SEPARATE:** Delivery/Live, Management Capture, History/Analytics y validación Web de la nueva cadena.

Los números de pruebas son evidencia de **logs locales comunicados**; la inspección de Git confirma contenido en HEAD, pero por sí sola no acredita ejecuciones de esos tests sobre checkout limpio del commit.
