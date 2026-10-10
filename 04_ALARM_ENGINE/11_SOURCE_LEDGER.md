# Alarm Engine — Source Ledger

Estado: **CURRENT — consolidación del incremento de publicación, WAL retention inicial y stress acceptance**. Revisión: 2026-10-10.

## Autoridades

```text
implementation: moragaga/atlanticus:main
verified HEAD: c3b8ed3b8de4bbafdaeeff4410d4daaa20bed1b4
contracts FACTS v4: b0c3a3d98a5c78a51321272bb3b0c7bc30494952
backend/runtime summary policy: 0fdf8f30
alarms/persistence WAL/checkpoints: f48c9dca
processes/alarm-runtime publication/stress: c3b8ed3b8de4bbafdaeeff4410d4daaa20bed1b4

canonical: moragaga/atlanticus-cannonical:main
HEAD leído antes de esta actualización: f34568a661b57fbeecfb0e6eda867bf5b613712d

decisions: moragaga/atlanticus-decisions:main
HEAD leído: 50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Los SHAs abreviados de commits intermedios identifican commits incluidos en el HEAD de implementación; no son hashes inventados. El baseline anterior de persistencia `758249d5fa35236b0ac9b990a393083b4463a507` (2026-10-08) queda como **HISTORICAL**.

## Código y contratos CURRENT

```text
backend/runtime/src/atlanticus/runtime/{definition,context,runner}.py
scopes/ada-contracts/alarms/src/ada/contracts/alarms/facts_stream.py
scopes/ada-contracts/alarms/src/ada/contracts/alarms/schemas/
  engine_committed_facts_stream.v4.schema.json
  engine_facts_export_cursor.v4.schema.json
scopes/ada-alarm-engine/alarms/persistence/src/ada/alarms/persistence/operational/
  journal.py
  incremental.py
  recovery_checkpoint.py
  store.py
scopes/ada-alarm-engine/processes/alarm-runtime/src/ada/processes/alarm_runtime/
  composition.py
  settings.py
  publication/output_batches.py
  publication/output_current.py
scopes/ada-alarm-engine/processes/alarm-runtime/stress/
  run_synthetic.py
  recovery_verification.py
  acceptance.py
  tests/
```

## Evidencia de qualification

- **VERIFIED remoto:** las rutas anteriores del incremento están presentes en `atlanticus:main` y FACTS exporter actual declara `SCHEMA_VERSION = 4`. `output_current.py` define `ada_alarm_engine_durable_current_state` v1.
- **VERIFIED por logs aportados localmente (2026-10-10):** 70 + 147 + 171 + 165 = **553 PASS**, con Ruff check de los archivos tocados. No representa full CI del repositorio.
- **VERIFIED por readjudicación de evidencia sintética local v3:** 7/7 controles, 10 minutos/3 alarmas, checkpoint sequence 10, un segmento retenido, 322 fact batches y dos grupos CURRENT. La ejecución no se repitió con el Acceptance Gate actualizado.
- **HISTORICAL:** `atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856` verificó Modeler/Delivery/Cosmos con contratos de Command Center anteriores; no cualifica el nuevo stream/current.
- **UNVERIFIED:** evaluadores productivos, continuidad con Modeler/Delivery a través del nuevo contrato, Azure/multi-host/CI remoto y eficacia final de retención/footprint FACTS.

## Conflictos y fronteras

El nuevo `ada_alarm_engine_durable_current_state` v1 **no es** el viejo `ada_command_center_engine_resolved_current_state` v1, aunque ambos reciban la etiqueta CURRENT. FACTS v4 **no es** FACTS batch v2/v3. El pipeline Modeler/Delivery histórico sigue existiendo como referencia pero la transición a los contratos nuevos permanece **OPEN**, sin inferir adapters ni migración automática.

No hay modificación de decisiones históricas o contratos frozen Live / Management / History-Analytics en este cierre.
