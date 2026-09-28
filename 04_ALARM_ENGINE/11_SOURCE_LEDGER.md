# Alarm Engine — Source Ledger

Estado: **AUDIT LEDGER / B2a + B2b INTEGRADOS EN MAIN / GATES LOCALES CLOSED / NUEVA INFRAESTRUCTURA UNVERIFIED**.

Fecha del corte: 2026-09-27. Todos los repositorios se consultaron en modo **READ ONLY**; el contenido de Git se distingue de los logs de validación proporcionados por el usuario y de las propuestas aún no implementadas.

## Autoridades verificadas

```text
Implementación MAIN : moragaga/atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5
Canonical vigente  : moragaga/atlanticus-cannonical@be2c424c44648e6488daae36d410cf425eed02b8
Decisions vigente  : moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

El HEAD final fue comunicado por el usuario y comprobado mediante lectura de la rama `main` y comparación con el checkpoint anterior. **No** se confunde con un HEAD futuro de canonical tras integrar estos reemplazos. Parte de los HEAD intermedios incluyen commits de otros frentes; los commits de archivos específicos siguientes fueron contrastados individualmente.

## Genealogía relevante del producto

```text
Pure B.2 inicial                                    9398786ae9af7c00de1bcca9d7a311fe9ef2155f
Strict routing                                     411aea44ac60c09d2b07ce41d34c3f378788b97b
Materialization proceso checkpoint                b600ca591b56d0924aed752dfae6e9fab2c6f1d6
Materialization local antes del lector            1076dfaab2537f2ccd4d7b3cc9df8dac245f534d
Lector compartido + adapter Runtime (A)          9693e2b791b34624d551c52821274231ae05f2af
Artefacto exacto + planning B1                    c8f23d91ae1cb817be55b4b812b22ffca518880e
B2a.1 commits de archivos Persistence             3c616dab38a80467359c48c05144389db0211b80
B2a.2 commits de archivos Persistence             e0578d3338138693430249803b6397e32f422227
B2b.1 commits de archivos Persistence             3ce75d87f7158f2cd70b53e6a99864b4c42bede9
B2b.1 checkpoint HEAD comunicado                 963c21d340be6bd157550d516ad32d97668fbc57
B2b.2 commits de archivos Runtime                ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5
```

Detalles confirmados: `3c616dab` modificó 13 archivos de Persistence B2a.1 y tests; `e0578d` modificó nueve archivos de Persistence B2a.2. `3ce75d8` modificó los nueve archivos de B2b.1 mientras el HEAD comunicado `963c21d` agregó posteriormente otro commit no relacionado de paquetes ZIP. `ebc7a8b` modificó exactamente cinco archivos de B2b.2; su parent directo `de3ae0...` añadió cambios a ADA Web/distribution **ajenos al scope** de Alarm Engine. No describir todo diff entre checkpoints como trabajo de alarmas.

## Archivos CURRENT contrastados

```text
scopes/ada-command-center/backend/
  alarms/materialization/
    src/ada_command_center/alarms/materialization/
      resolver.py                       # resolver puro de B.2
      codec.py, runtime.py, delivery.py # pareja Rn/Cn
      local_reader.py                   # READY publicado/exacto
      artifact_reference.py            # pin exacto B1
  alarms/persistence/
    src/ada_command_center/alarms/persistence/
      configuration_adoption.py        # V1, V2, GroupCommitReference
      journal.py, models.py, store.py  # WAL, durable/materialized, recovery
      effective_head.py                # alarm-effective-head.v1
      paths.py, serialization.py       # layout y JSON/hash
    commented/...                      # espejos pedagógicos equivalentes
    tests/
      test_configuration_adoption.py
      test_configuration_adoption_v2.py
      test_effective_head.py
  processes/alarms-materialization/
    src/ada_command_center/processes/alarms_materialization/
      acquisition.py, qualification.py, publication.py
      job.py, composition.py
  processes/alarms-runtime/
    src/ada_command_center/processes/alarms_runtime/
      local_configuration.py           # READY/exacto + EFFECTIVE exacto
      adoption.py                      # planificador B1
      adoption_execution.py            # ejecutor parcial anterior a B2
      job_composition.py               # recovery barrier e iteration hook
      composition.py, durability.py    # commits de grupo y fencing
      session.py, iteration.py, cycle.py
    tests/
      test_adoption_plan.py
      test_local_configuration_reader.py
      test_effective_local_configuration.py
```

Los paquetes relevantes se conservan en versión `1.0.0` predespacho. El Project adopta Python 3.14.7 de forma general, pero los `pyproject.toml` de estos paquetes Command Center fijan `==3.14.2`. Los logs de este corte usan `uv run --python 3.14.2`; no modificar metadata transversal en este hito. La existencia histórica de varios `uv.lock` se conoce, pero no se determinó si deben desaparecer; no declararlos legacy ni eliminarlos incidentalmente.

## Pruebas locales comunicadas por el usuario

| Incremento | Evidencia del usuario | Alcance probado |
|---|---|---|
| A | Materialization 49 PASS, proceso 43 PASS, Runtime 23 PASS, Ruff/format/build PASS | Publicación/lectura local y adaptación inicial. |
| B1 | Materialization 62 PASS; Runtime 40 PASS; proceso 43 PASS; Ruff/format/build de paquetes relevantes PASS | Pin B1 y planificación, **no** ejecución completa. |
| B2a.1 | Persistence 57 PASS; Ruff/format PASS; `git diff --check` PASS; wheel previo al último format PASS | Adopción global V1, recovery local. |
| B2a.2 | 18 pruebas específicas PASS tras corregir **test** cross-hour; suite Persistence PASS; Ruff/format/diff PASS; wheel/sdist de log previo PASS | V1/V2, batches y crash/recovery. |
| B2b.1 | 19 específicas + suite Persistence **94 PASS**; Ruff/format/diff y wheel/sdist PASS | Proyección EFFECTIVE y recovery local. |
| B2b.2 | 15 específicas + suite Runtime **55 PASS**; Ruff/format/diff y wheel/sdist PASS | Lector EFFECTIVE exacto y fail-closed local. |

B2a.2: fixture cronológica del test fue la única corrección detectada, no bug productivo demostrado. B2b.2: primera ejecución de CLI ocurrió fuera del workspace, no encontró Ruff/pytest/pyproject; segunda desde `scopes/ada-command-center/backend` PASS. En B2b.1 hubo warning README de sdist, sin bloquear el build y sin requerir README nuevo.

Estos datos son **VERIFIED de logs proporcionados** y se comprobó que el código integra en los commits citados. Son **UNVERIFIED como CI/rerun limpio sobre el SHA final**; tampoco prueban producción ni volumen físico multi-host.

## Decisions y discrepancias documentadas

La genealogía histórica está en `atlanticus-decisions:main/alarm_decisions/`, especialmente `R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md` y decisiones B.2 de proyección/publicación. B.1 desea `evaluator_key` y `kind` COMPATIBLE y cambio `priority_group` con migración de origen/destino; planificador actual los rechaza. El ejecutor sigue sin cubrir `ADDED`, `ENABLED` y `REMOVED` desde source no ejecutable; B2a/B2b son infraestructura disponible, **no** reparación automática del ejecutor.

Canonical `be2c424...` es anterior a los incrementos de este cierre; sus afirmaciones de global adoption/Effective Head PLANNED no representan `atlanticus@ebc7a8b...`. Los presentes archivos reemplazan esa descripción **sólo después de su integración manual**.

## Límites y siguiente corte

**OPEN / UNVERIFIED:** E2E operativo, Cosmos/Blob reales, qualification GREEN real, volumen multi-host, migración real de historia group-only si existe. **PLANNED B2c:** ejecución/adopción integral sobre contratos V1/V2/EFFECTIVE actuales. **SEPARATE:** Delivery/Live, Management Capture, History/Analytics, cambios Python transversales y UX routing visual.

El siguiente chat debe releer `atlanticus:main` y `atlanticus-decisions:main` y revisar canonical tras integración humana. No efectuar operaciones Git remotas desde el asistente.
