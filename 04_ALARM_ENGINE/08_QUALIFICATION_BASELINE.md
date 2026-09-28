# Alarm Engine — Qualification Baseline

Estado: **CURRENT — evidencia histórica R3.5 preservada; A/B1/B2a/B2b y B2c.5c/B2c.5d con gates locales; CI y E2E operativo UNVERIFIED**. Corte documental 2026-09-28; implementación auditada `atlanticus@a799dc15105d3e037f36ab77129ef0cfa8999013`.

**Regla de evidencia:** la lectura remota comprueba presencia de código/commits; las cifras de pruebas son logs locales aportados por el usuario, no una ejecución remota ni una recalificación de Docker. No mezclar benchmarks históricos con el árbol de B2c.

## Campaña histórica R3.5 — CLOSED histórica, no reejecutada

Genealogía verificable en `atlanticus-decisions:main/alarm_test/`: E-008 Source Unavailable / CACHE_FALLBACK (planner v1.0.92), E-009 Invalid Source Candidate (v1.0.95), E-010 Lease Lost After WAL Before Cache (v1.0.103: primera corrida ABORTED/NOT ADJUDICATED y cierre posterior con harness corregido), E-011 Cache Promotion Failure (v1.0.110: finding del adjudicador/harness, no defecto de producto demostrado), E-012 Drain Under Workload (v1.0.117: 1000 alarmas, 10 grupos, 480 management requests y 480 decisions, con corrección del finding de drain), F-001 Soak 500 Local 30m (v1.0.122), F-002 Soak 1000 Local 30m (v1.0.126), F-007 Physical/Docker/dataset bank (v1.0.135).

### F-010 Final Docker Qualification histórica — CLOSED PASS/GREEN

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
management      480/480 requests; 480/480 decisions
adoption        compatible 1000
threshold       0.50 -> 0.75
```

Sin product findings abiertos en la campaña histórica. **F-010 no valida los paquetes ni interfaces de B2c actuales**; no reutilizar como E2E nuevo. Los documentos originales de decisions conservan detalles de ejecución/harness y criterios de cierre.

## Materialization B.2, routing y primeros checkpoints

- Pure B.2 `atlanticus@9398786ae9af7c00de1bcca9d7a311fe9ef2155f`: resolver puro, qualification, routing C1/C2/C3, validación visual, Messages y reappearance; en su momento 31 tests y lint GREEN.
- Strict routing `atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b`: logs locales Domain 56, Materialization 49, Web Configuration 114, Ruff/format PASS por componente; host/browser real UNVERIFIED.
- Incremento A, lector/publicación local: Materialization 49, proceso 43, Runtime 23; Ruff/format/wheels comunicados PASS, tras correcciones de fixtures/import/formato. Checkout de referencia `9693e2b791b34624d551c52821274231ae05f2af`.
- B1, pin exacto/plan de adopción: Materialization 62, Runtime 40, proceso 43; Ruff/format y builds relevantes PASS según logs delimitados. Código `atlanticus@c8f23d91ae1cb817be55b4b812b22ffca518880e`.

## B2a/B2b — gates locales previos

| Incremento | Evidencia local histórica | Alcance probado |
|---|---|---|
| B2a.1 | Persistence 57 PASS, Ruff/format PASS, diff PASS; wheel comunicado antes del último formato | Adopción V1 global sin grupos, recovery. |
| B2a.2 | 18 específicas PASS tras corregir fixture cronológica de test; suite Persistence y Ruff/format/diff PASS; wheel/sdist de log previo | Adopción V2 con grupos, lote y replay. |
| B2b.1 | 19 específicas, Persistence **94 PASS**, Ruff/format/diff/wheel/sdist PASS | Effective Head recuperable local. |
| B2b.2 | 15 específicas, Runtime **55 PASS**, Ruff/format/diff/wheel/sdist PASS | Lector EFFECTIVE exacto y fail-closed local. |

Referencias de código: B2a.1 `3c616dab38a80467359c48c05144389db0211b80`, B2a.2 `e0578d3338138693430249803b6397e32f422227`, B2b.1 `3ce75d87f7158f2cd70b53e6a99864b4c42bede9`, B2b.2 `ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`. La suite original de B2a.2 tuvo una fixture cross-hour corregida, no un defecto de producto demostrado. El warning histórico sdist sobre README no exige README nuevo en un incremento parcial.

## B2c.5c — VERIFIED de logs locales, CLOSED tras integración

El usuario aplicó la integración de registro/lectura de fuentes y requisitos por evaluador y corrigió el test de arquitectura de frontera física y sincronización de espejos tras Ruff. Evidencia compartida:

```text
pytest processes/alarms-runtime/tests integration_tests            119 PASS
pytest alarms/core/tests alarms/persistence/tests
       processes/alarms-materialization/tests
       processes/alarms-runtime/tests integration_tests             436 PASS
ruff check (src/runtime, tests, integration_tests)                 PASS
ruff format --check (mismo conjunto)                               33 archivos PASS
git diff --check                                                    sin hallazgos
```

El usuario posteriormente comunicó `atlanticus@672ed047f59459034d2fde23d05427e234bd01d5` y ese SHA fue comprobado remotamente; la ejecución local usa Python `--python 3.14.2` según metadata existente. **No** se probó lectura física de datasets productivos ni CI sobre checkout limpio.

## B2c.5d — VERIFIED de logs locales, CLOSED tras integración

Primera versión de ejemplo/catálogo: 6 pruebas específicas y **442 pruebas de regresión** PASS; Ruff lint PASS. Había un archivo de test pendiente de formato y se descartó cualquier gate de formato definitivo hasta corregirlo. Después se trasladó la lógica a `catalog/examples/threshold`, se dejó el catálogo productivo vacío y se ejecutó:

```text
pytest test_example_threshold_catalog.py test_example_threshold_cycle.py    7 PASS
pytest alarms/core/tests alarms/persistence/tests
       processes/alarms-materialization/tests
       processes/alarms-runtime/tests integration_tests                443 PASS
ruff check processes/alarms-runtime                                  PASS
ruff format --check src + tests                                     40 archivos PASS
git diff --check                                                    sin hallazgos
```

El usuario informó el SHA final `a799dc15105d3e037f36ab77129ef0cfa8999013`; la lectura de `atlanticus:main` confirmó ese HEAD y los archivos del traslado, tests y registro vacío. **UNVERIFIED:** repetición de estas suites en checkout aislado posterior al commit, build/wheel específico posterior al traslado, CI, despliegue o datos reales.

## Pendientes de qualification — no degradar el significado de CLOSED

- **UNVERIFIED / condicionado por entorno:** cobertura operacional real por fuente/partición, workers con datasets preparados, Cosmos/Blob reales, volumen físico multi-host, fallos de red y E2E de despliegue.
- **UNVERIFIED / separado:** productores operativos de qualification de evaluadores y Tool GREEN; el JSON controlado y las suites unitarias no son el producto operacional.
- **PLANNED B2c.6:** tests de la composición/arranque mínimo utilizando interfaces actuales y datos controlados; no diseñar benchmarks físicos ni ampliar scope durante el cierre documental.
