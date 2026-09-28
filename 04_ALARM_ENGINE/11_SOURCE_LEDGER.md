# Alarm Engine — Source Ledger

Estado: **CURRENT / ledger de cierre B2c.5c + B2c.5d; base histórica B.2/B2a/B2b preservada como referencias; tests físicos/CI UNVERIFIED**. Fecha: 2026-09-28. El Git del asistente permanece READ ONLY.

## Autoridades contrastadas

```text
Implementación a cierre: moragaga/atlanticus@a799dc15105d3e037f36ab77129ef0cfa8999013
Decisions al corte   : moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical de base    : moragaga/atlanticus-cannonical@46877f174513b2475f17b7dc739cd43951fa4ed0
```

La comprobación del último HEAD se hizo contra el branch `main` y el contenido concreto del catálogo/registro. Los recuentos de pruebas proceden de logs aportados por el usuario; **no** se ejecutaron CI ni `pytest` en el checkout Git remoto. El commit a799 contiene cambios relativos al traslado de ejemplos; no adjudicar a Alarm Engine commits de otras áreas entre checkpoints.

## Genealogía de implementación relevante

| Hito | SHA / referencia |
|---|---|
| Pure B.2 inicial | `9398786ae9af7c00de1bcca9d7a311fe9ef2155f` |
| Strict routing | `411aea44ac60c09d2b07ce41d34c3f378788b97b` |
| Materialization proceso | `b600ca591b56d0924aed752dfae6e9fab2c6f1d6` |
| Salida Materialization local | `1076dfaab2537f2ccd4d7b3cc9df8dac245f534d` |
| Lector compartido/adaptador inicial A | `9693e2b791b34624d551c52821274231ae05f2af` |
| Artefacto exacto/planning B1 | `c8f23d91ae1cb817be55b4b812b22ffca518880e` |
| Persistence B2a.1 V1 | `3c616dab38a80467359c48c05144389db0211b80` |
| Persistence B2a.2 V2 | `e0578d3338138693430249803b6397e32f422227` |
| B2b.1 Effective Head | `3ce75d87f7158f2cd70b53e6a99864b4c42bede9` |
| B2b.2 lector Runtime exacto | `ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5` |
| B2c.5c fuentes/requisitos + validación local | `672ed047f59459034d2fde23d05427e234bd01d5` |
| B2c.5d catálogo/ejemplo inicial | `f5aeea997cf3672cd96fef104be83ab6b4f440b7` |
| B2c.5d traslado final a examples | `a799dc15105d3e037f36ab77129ef0cfa8999013` |

La genealogía original R3.5/F-010 no se sustituye con B2c y reside en `atlanticus-decisions:main/alarm_test/` y `08_QUALIFICATION_BASELINE.md`.

## Archivos CURRENT comprobados

```text
scopes/ada-command-center/
  domain/alarms/src/ada_command_center/domain/alarms/
    definition.py, configuration.py, models.py
  backend/alarms/core/src/ada_command_center/alarms/core/
    evaluation.py, models.py, lifecycle.py, priority.py, evidence.py, commit.py, deactivation.py
  backend/alarms/persistence/src/ada_command_center/alarms/persistence/
    configuration_adoption.py, effective_head.py, journal.py, store.py
  backend/alarms/materialization/src/ada_command_center/alarms/materialization/
    resolver.py, artifact_reference.py, local_reader.py, runtime.py, delivery.py
  backend/processes/alarms-materialization/src/ada_command_center/processes/alarms_materialization/
    acquisition.py, qualification.py, publication.py, job.py, composition.py
  backend/processes/alarms-runtime/src/ada_command_center/processes/alarms_runtime/
    adoption.py, adoption_execution.py, configured_iteration.py, local_configuration.py,
    job_composition.py, process.py, operational_runner.py, cycle.py, session.py,
    iteration.py, source_adapter.py, source_reader.py
    catalog/__init__.py, catalog/registry.py
    catalog/examples/__init__.py
    catalog/examples/threshold/__init__.py, evaluator.py, requirements.py
  backend/processes/alarms-runtime/commented/ada_command_center/processes/alarms_runtime/
    catalog/registry.py, catalog/examples/threshold/*.py
  backend/processes/alarms-runtime/tests/
    test_parameterized_sources.py, test_alarm_source_reader.py,
    test_example_threshold_catalog.py, test_example_threshold_cycle.py,
    test_architecture.py, test_commented_mirror.py
  web/alarms/configuration/src/ada_command_center/web/alarms/configuration/web/
    layout.py, callbacks.py
```

En `a799dc1`, `catalog/registry.py` devuelve `AlarmEvaluatorRegistry(contracts=())`, no registra el ejemplo. Ejemplo real identificado por `(family_key='mina',evaluator_key='threshold')` sólo bajo imports explícitos de tests. `process.py` requiere los puertos `evaluator_registry` y `source_loader`, por lo que la futura composición debe inspeccionar/reutilizar interfaces ya presentes. `source_reader.py` soporta datasets parquet por aplicación; sin ambiente de datos reales este hecho no constituye prueba física.

Web: `layout.py` presenta `default_deactivation.max_duration_hours` mediante `_number_field`; `domain/alarms/definition.py` acepta máximo habilitado entero 1..12. No existe evidencia de un contrato CURRENT para opción `fin del turno`; es un OPEN identificado por el usuario.

## Evidencia local del usuario, delimitada

| Hito | Evidencia comunicada |
|---|---|
| A | Materialization 49, proceso 43, Runtime 23 PASS y gates Ruff/format/build delimitados. |
| B1 | Materialization 62, Runtime 40, proceso 43 PASS, gates de los paquetes afectados. |
| B2a.1 | Persistence 57 PASS, Ruff/format/diff PASS. |
| B2a.2 | 18 específicas PASS, suite Persistence y Ruff/format/diff PASS tras corregir fixture de test. |
| B2b.1 | 19 específicas y Persistence 94 PASS, Ruff/format/wheel/sdist PASS. |
| B2b.2 | 15 específicas y Runtime 55 PASS, Ruff/format/wheel/sdist PASS. |
| B2c.5c | Runtime+integration **119 PASS**; Core+Persistence+Materialization+Runtime+integration **436 PASS**; Ruff lint PASS, 33 archivos formateados, diff PASS. |
| B2c.5d antes de traslado | 6 específicas, regresión **442 PASS**, lint PASS; formateo de un test seguía pendiente y no era gate completo. |
| B2c.5d tras traslado | **7 específicas + 443 de regresión PASS**, Ruff lint PASS, **40 archivos format PASS**, diff PASS, antes del commit final. |

**UNVERIFIED:** test rerun sobre checkout limpio de `a799dc1`, CI, wheel específico tras mover examples, todos los datasets físicos, Cosmos/Blob/Azure reales y volumen multi-host.

## Discrepancias y límites

- **CURRENT vs decisions B.1:** `adoption.py` sigue rechazando `evaluator_key`, `kind` o `priority_group` que en decisiones históricas se quieren compatibles o migrables; pendiente separado, no corregido.
- **CURRENT vs canonical de partida:** el corte documental `46877f...` termina con B2c PLANNED o sólo B2b; actualizar a través de estos reemplazos no implica que ya estén integrados remotamente.
- **Python baseline:** Project 3.14.7 objetivo vs metadata Command Center `==3.14.2`; no editar como efecto lateral.
- **OperationalScope PI:** el contrato actual carece de semana operacional genérica; `ShiftScope.CURRENT_WEEK` no equivale y FABRICA_PLANES WEEKLY es fuente distinta. KPI Runtime pendiente separado.
- **OPEN Web:** deactivation fin de turno requiere contrato funcional, no sólo cambiar número por selector.

## Próximo corte

**B2c.6 — sólo auditar y acordar wiring del proceso** con los puertos existentes, catálogo productivo vacío y sources actuales; no mezclar qualification productiva, Web, Live, History ni migraciones ajenas.
