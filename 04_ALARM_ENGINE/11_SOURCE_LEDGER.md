# Alarm Engine — Source Ledger

Estado: **CURRENT — inventario de fuentes y evidencia hasta B2c.7d**. Corte 2026-09-28. Git del asistente: SOLO LECTURA. No atribuir verificaciones de una generación a otra ni tratar logs como CI remoto.

## 1. Referencias del corte

```text
Implementation HEAD del hito verificado   c67fcb5b105cc561c16719a8bca4ea5aa74c3fae
Implementation actual main leído          bc1d73742bcb04eb495bbbb1725a8ad23d4eff38 (commit posterior sólo ADA Generic)
Decisions main leído                       50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical main anterior al reemplazo      5558cf9d92d9b21758500024b6099011416d78da
```

**Verificación remota adicional de este cierre:** Git confirmó el objeto `c67fcb5b105cc561c16719a8bca4ea5aa74c3fae` y su diff de ocho archivos. Se leyó el contrato FACTS v2 (`schema_version=2`), productor `output_batches.py` (`previous_batch`) y receptor `receiver.py` (validación de cadena) directamente en ese commit; también se verificó la presencia del test de integración y del schema CURRENT v1. El HEAD actual `bc1d73742bcb04eb495bbbb1725a8ad23d4eff38` está un commit más adelante sólo por ADA Generic. Los resultados 32/162 PASS y Ruff son logs del usuario **anteriores al commit**: no implican ejecución de CI sobre checkout limpio.

## 2. Genealogía histórica preservada

| Hito | SHA / referencia |
|---|---|
| Pure B.2 inicial | `9398786ae9af7c00de1bcca9d7a311fe9ef2155f` |
| Strict routing | `411aea44ac60c09d2b07ce41d34c3f378788b97b` |
| Materialization proceso | `b600ca591b56d0924aed752dfae6e9fab2c6f1d6` |
| Salida Materialization local | `1076dfaab2537f2ccd4d7b3cc9df8dac245f534d` |
| Lector compartido/adaptador inicial A | `9693e2b791b34624d551c52821274231ae05f2af` |
| Artefacto exacto/planning B1 | `c8f23d91ae1cb817be55b4b812b22ffca518880e` |
| Persistence B2a.1 adopción V1 | `3c616dab38a80467359c48c05144389db0211b80` |
| Persistence B2a.2 adopción V2 | `e0578d3338138693430249803b6397e32f422227` |
| B2b.1 Effective Head | `3ce75d87f7158f2cd70b53e6a99864b4c42bede9` |
| B2b.2 lector Runtime exacto | `ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5` |
| B2c.5c fuentes/requisitos | `672ed047f59459034d2fde23d05427e234bd01d5` |
| B2c.5d catálogo/example final | `a799dc15105d3e037f36ab77129ef0cfa8999013` |
| B2c.7a CURRENT/FACTS inicial | `efe231d61c9d5a6f4eca1e3f22a201a9b3c1861b` (usuario, commit local) |
| B2c.7b input receiver | `94f26213ca28b550baf53d8ee34e34da7538ad17` (usuario y Git remoto leído) |
| B2c.7c prueba integración | `199fc0f4ff543fef6ff204a892370a53bf905423` (usuario, Git verificado) |
| B2c.7d cadena FACTS v2 | `c67fcb5b105cc561c16719a8bca4ea5aa74c3fae` (usuario, Git verificado) |

Para R3.5/F-010 consultar los documentos originales de `atlanticus-decisions:main/alarm_test/`; los benchmarks de esa campaña son históricos, no qualification de B2c.7.

## 3. Archivos implementados relevantes

```text
scopes/ada-command-center/
  domain/alarms/src/ada_command_center/domain/alarms/
    definition.py, configuration.py, models.py
  backend/alarms/core/src/ada_command_center/alarms/core/
    evaluation.py, models.py, lifecycle.py, priority.py, evidence.py,
    commit.py, deactivation.py
  backend/alarms/materialization/src/ada_command_center/alarms/materialization/
    resolver.py, artifact_reference.py, local_reader.py, runtime.py, delivery.py
  backend/alarms/persistence/src/ada_command_center/alarms/persistence/
    configuration_adoption.py, effective_head.py, journal.py, store.py
  backend/alarms/contracts/
    engine_resolved_current_state.v1.schema.json
    engine_committed_facts_batch.v1.schema.json    # historical artifact; not v2 runtime adapter
    engine_committed_facts_batch.v2.schema.json    # CURRENT runtime
  backend/processes/alarms-materialization/src/ada_command_center/processes/alarms_materialization/
    acquisition.py, qualification.py, publication.py, job.py, composition.py
  backend/processes/alarms-runtime/src/ada_command_center/processes/alarms_runtime/
    process.py, application.py, bootstrap.py, configured_iteration.py,
    operational_runner.py, cycle.py, session.py, source_adapter.py, source_reader.py,
    catalog/registry.py, catalog/examples/threshold/*,
    publication/output_current.py, publication/output_batches.py
  backend/processes/alarms-delivery/src/ada_command_center/processes/alarms_delivery/
    receiver.py, job.py, settings.py, bootstrap.py
  backend/processes/alarms-runtime/commented/.../publication/
  backend/processes/alarms-delivery/commented/.../
  backend/processes/alarms-runtime/tests/
    test_output_batches.py, test_output_current.py,
    test_example_threshold_cycle.py, test_configured_iteration.py
  backend/processes/alarms-delivery/tests/
    test_receiver.py, test_job.py, test_engine_delivery_integration.py
  backend/pyproject.toml, backend/uv.lock
```

El registro de evaluadores productivo continúa vacío; el ejemplo se importa expresamente. La dependencia `atlanticus-state==1.0.0` requerida por publicación se añadió a Engine y corrigió el test explícito de dependencias. Ambos jobs están en el workspace uv; todavía hay que inspeccionar su empaquetado/distribución real del HEAD final.

## 4. Evidencia de B2c.7 delimitada

```text
B2c.7a  137 PASS / 1 SKIPPED; Ruff + format + diff PASS
B2c.7b  151 PASS / 1 SKIPPED; Ruff + format + diff PASS
B2c.7c    1 PASS integración; 152 PASS / 1 SKIPPED; Ruff + format PASS
B2c.7d   32 PASS específicas; 162 PASS / 1 SKIPPED; Ruff + 61 format PASS
```

B2c.7c usa Engine real y muestra controlada sobre `tmp_path`; B2c.7d aporta tests de `previous_batch`, missing first/intermediate, tampering, replay/recovery de cadena y rechazo de estado v1. El reporte del usuario no adjunta resultado de CI, wheel final o Docker independiente. No identificar el test SKIPPED sin ejecutar `pytest -rs`.

## 5. Contratos y discrepancias

- FACTS v2 strict del productor y receptor reemplaza semánticamente el runtime v1; **WAL adoption V1/V2 siguen ambos CURRENT**.
- `current/latest.json` reemplazable y completo, FACTS archivos inmutables; schemas productivos en Git.
- Cursor Engine y cursor Delivery tienen ownership distinto; ausencia de salida no se convierte en snapshot vacío o hecho ficticio.
- READ ONLY: Git `atlanticus` ahora contiene el commit del hito; este ledger no implica que se efectuó push a decisions/canonical ni que se ejecutó CI de este commit.
- B.1 (evaluator/kind/group, Special Cascade), inactive Messages, target/routing, fin de turno y Python 3.14.7/3.14.2 permanecen conflictos/OPEN documentados en `09_DECISION_INDEX.md` y `10_OPEN_ITEMS.md`.

**Próxima lectura obligatoria:** confirmar que el árbol remoto disponible sigue incluyendo c67fcb5 y distinguir avances ajenos al foco; auditar `pyproject.toml`, `uv.lock`, scripts, assets y volumen real antes de cualquier modificación del job.
