# Alarm Engine — Source Ledger

Estado: **CURRENT — 13F.2c durable Runtime closeout**. Revisión: 2026-10-08.

## Autoridades auditadas

```text
implementation repository: moragaga/atlanticus
branch: main
commit: 758249d5fa35236b0ac9b990a393083b4463a507
commit timestamp: 2026-10-08T15:17:31Z

canonical baseline: moragaga/atlanticus-cannonical
branch: main
commit previo a esta actualización: e950e2d0e2817ba25789765e9dbb0ef790866af3

decisions historical repository: moragaga/atlanticus-decisions
branch: main
commit auditado: 50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

La implementación publicada es autoridad de código. La fuente histórica de decisiones conserva rationale y qualification; no sustituye el `main` actual.

## Superficies actuales verificadas por lectura remota

```text
scopes/ada-alarm-engine/alarms/core/
scopes/ada-alarm-engine/alarms/materialization/
scopes/ada-alarm-engine/alarms/persistence/src/ada/alarms/persistence/operational/
  store.py
  models.py
  configuration_adoption.py
  configuration_rebase.py
  core_commit_bridge.py
  lifecycle_snapshot.py
scopes/ada-alarm-engine/processes/alarm-runtime/src/ada/processes/alarm_runtime/
  bootstrap.py
  composition.py
  job.py
  lifecycle.py
  cycle.py
  durable_recovery.py
  durable_commit.py
  durable_adoption.py
  operational_adoption.py
  catalog/registry.py
```

Pruebas especialmente relevantes:

```text
alarms/persistence/tests/operational/test_recovery.py
alarms/persistence/tests/operational/test_fencing.py
alarms/persistence/tests/operational/test_configuration_adoption_v2.py
alarms/persistence/tests/operational/test_configuration_rebase.py
processes/alarm-runtime/tests/test_durable_recovery.py
processes/alarm-runtime/tests/test_durable_cycle_commit.py
processes/alarm-runtime/tests/test_durable_adoption.py
processes/alarm-runtime/tests/test_operational_adoption.py
```

Las rutas de pruebas anteriores son relativas a `scopes/ada-alarm-engine/`.

## Evidencia y clasificación

- **VERIFIED (lectura remota):** `main` contiene el Runtime y contratos de persistencia/adopción, más los tests enumerados.
- **VERIFIED (logs locales del usuario):** el 2026-10-08 se reportaron 471 PASS, Ruff check PASS, Ruff format PASS, `uv lock --check` PASS y `git diff --check` PASS.
- **INFERRED con respaldo en el grafo de composición:** el nuevo Runtime no conecta el exportador CURRENT/FACTS histórico.
- **HISTORICAL:** E2E local en `atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856`: READY/EFFECTIVE, ACTIVE/PREDOMINANT, CURRENT/FACTS, Modeler/Delivery y read-back Cosmos.
- **UNVERIFIED:** qualification física de la nueva composición y ejecución con evaluadores productivos.

## Procedencia de la actualización canónica

Este cambio documental refleja el cierre de auditoría 13F.2c.3d. No modifica `atlanticus:main`, decisiones remotas ni crea nueva evidencia física. El commit canónico definitivo se anotará únicamente después de que el usuario publique este paquete; no se anticipa su SHA.
