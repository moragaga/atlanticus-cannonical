# Alarm Engine — Source Ledger

Estado: **AUDIT LEDGER / MATERIALIZATION LOCAL + RUNTIME ADOPTION B1 CHECKPOINT / 2026-09-27**

## Autoridad verificada en modo lectura

```text
Implementation HEAD : moragaga/atlanticus@c8f23d91ae1cb817be55b4b812b22ffca518880e
Canonical baseline  : moragaga/atlanticus-cannonical@58241ddb6db5adbd2e783c7ec9f456f1bda5a321
Historical decisions: moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

El HEAD comunicado por el usuario se corroboró mediante lectura remota de Git; canonical y decisions también. La comparación Git entre `atlanticus@9693e2b...` y `@c8f23d9...` confirmó **un commit adicional** con cambios B1. Los reemplazos de este paquete son archivos locales para integrar manualmente: no fueron escritos en Git por el asistente y no se debe asumir todavía nuevo HEAD canónico.

## Genealogía de commits relevante

```text
Pure B.2 inicial                           9398786ae9af7c00de1bcca9d7a311fe9ef2155f
Strict routing completo                   411aea44ac60c09d2b07ce41d34c3f378788b97b
Executable materialization checkpoint      b600ca591b56d0924aed752dfae6e9fab2c6f1d6
Materialization local previa a lector     1076dfaab2537f2ccd4d7b3cc9df8dac245f534d
Local shared reader + Runtime adapter (A) 9693e2b791b34624d551c52821274231ae05f2af
Artifact exacto + planning B1             c8f23d91ae1cb817be55b4b812b22ffca518880e
```

Entre `b600ca...` y `c8f23d...` Git reportó cinco commits; los títulos genéricos de commit no sustituyen la inspección de archivos. Las siguientes rutas se comprobaron en el HEAD final.

## Rutas y contratos CURRENT contrastados

```text
scopes/ada-command-center/backend/
  pyproject.toml                          # workspace, versión 1.0.0
  uv.lock                                 # workspace presente
  alarms/materialization/
    pyproject.toml                        # 1.0.0, Python ==3.14.2
    src/ada_command_center/alarms/materialization/
      resolver.py                         # resolve_alarm_configuration puro
      runtime.py / delivery.py
      codec.py                            # codec compartido Runtime/Delivery
      local_reader.py                     # lectura READY/exacta, validación/integridad
      artifact_reference.py               # AlarmConfigurationArtifactRef B1
    tests/test_artifact_reference.py
  processes/alarms-materialization/
    pyproject.toml                        # 1.0.0, Python ==3.14.2
    src/ada_command_center/processes/alarms_materialization/
      acquisition.py / candidate.py / qualification.py
      job.py / composition.py / publication.py
      settings.py / bootstrap.py / __main__.py
    tests/test_executable_process.py
  processes/alarms-runtime/
    pyproject.toml                        # 1.0.0, Python ==3.14.2
    src/ada_command_center/processes/alarms_runtime/
      session.py / adoption.py / adoption_execution.py
      local_configuration.py / job_composition.py
      composition.py / durability.py
    tests/test_local_configuration_reader.py
    tests/test_adoption_plan.py
```

El árbol contiene también `alarms/materialization/uv.lock` y `processes/alarms-materialization/uv.lock`, además del `backend/uv.lock` del workspace. Su coexistencia está verificada, pero **no** se determinó si los locks individuales son obsoletos ni se autorizó su eliminación en B1; no calificarlos automáticamente como legacy.

El proceso usa `CosmosAlarmConfigurationProjectionStore` sólo para adquirir proyección; salida `LocalAlarmMaterializationResultStore`. **No** existe el antiguo codec `processes/alarms-materialization/.../codec.py` ni `CosmosAlarmMaterializationResultStore` como salida en este árbol. Runtime puede leer READY actual o exacto, construir la revisión B1, planificar; todavía no adopta globalmente ni publica EFFECTIVE. B1 modifica `adoption.py` y `local_configuration.py`, **no** `adoption_execution.py` ni el WAL.

`backend/alarms/materialization` conserva pureza de su *resolver* pero ahora también alberga `local_reader.py` con I/O. No ocultar esta diferencia con la descripción histórica del paquete puro.

## Evidencia local comunicada por el usuario

### Incremento A — validación anterior a `9693e2b...`

```text
uv sync --python 3.14.2 --all-packages           PASS
materialization compartido                      49 unit PASS / Ruff PASS / format PASS / wheel PASS
materialization process                        43 unit PASS / Ruff PASS / format PASS / wheel PASS
runtime local reader                            7 unit PASS
runtime completo                               23 unit PASS / Ruff PASS / format PASS / wheel PASS
```

### Incremento B1 — anterior a `c8f23d9...`, incluida corrección Ruff

```text
workspace: uv sync --python 3.14.2 --all-packages PASS (56 paquetes resueltos)
materialization: tests/test_artifact_reference.py 13 PASS, suite 62 PASS
materialization: Ruff PASS / format PASS / wheel 1.0.0 PASS
runtime: tests/test_adoption_plan.py 17 PASS, suite 40 PASS
runtime: Ruff PASS / format PASS tras SIM102 y formateo, wheel 1.0.0 PASS
materialization process: suite 43 PASS, Ruff PASS, format PASS
```

Es evidencia **VERIFIED de logs proporcionados** y confirmación de presencia de código en Git, no un CI/re-run limpio en el HEAD exacto ni prueba E2E. No generar un SHA de árbol de trabajo a partir del log; el commit remoto actual contiene los cambios descritos.

## Límites y conflictos constatados

- Canonical previo `58241d...` aún marca salida local/lector/ADDED/ENABLED como PLANNED; estos reemplazos corrigen esas secciones, sin borrar historia de decisiones.
- `atlanticus-decisions` documenta como objetivo compatibilidad para cambios de `evaluator_key`/`kind` y migración de priority group, mientras Runtime aún los rechaza.
- `is_adoptable` vs `requires_execution_upgrade`: planner B1 admite nuevas disposiciones, executor anterior no las ejecuta con garantía; no confundir PASS de tests de planificador con éxito de adopción.
- Futura adopción global, Effective Head, Delivery local exacto, qualification real, Cosmos/Blob E2E y propiedades del volumen físico multi-host siguen UNVERIFIED/PLANNED.
- Baseline Python de Project `3.14.7` vs pin actual `==3.14.2` permanece en frente transversal independiente.

## Condición de actualización

Una vez que el usuario integre estos reemplazos en `atlanticus-cannonical`, corroborar HEAD/diff por Git solo lectura. Después abrir un chat técnico único para el **diseño** de Runtime Adoption durable; cualquier nueva implementación requiere consenso/autorización.
