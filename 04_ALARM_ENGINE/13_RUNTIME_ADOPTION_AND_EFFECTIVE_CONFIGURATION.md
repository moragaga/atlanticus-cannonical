# Alarm Engine — Runtime Adoption and Effective Configuration

Estado: **PARTIAL IMPLEMENTATION IN CURRENT ALARMS-RUNTIME / GLOBAL EFFECTIVE INTEGRATION PLANNED AFTER MATERIALIZATION LOCAL**

Corte inspeccionado: `atlanticus@b600ca591b56d0924aed752dfae6e9fab2c6f1d6`; canonical anterior `772d15078c97802d58d8b658b0d5d5b928fa2ed5`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.

## Frontera congelada

```text
READY != EFFECTIVE
AlarmResolutionKey(alarm_configuration_revision, confirmed_tool_catalog_revision)
```

B.2 READY aporta `RuntimeAlarmConfiguration` y `DeliveryAlarmConfiguration` de la misma key. Materialization publicará la pareja **en volumen local** una vez implementada la corrección; no convierte ningún candidato en EFFECTIVE. Los consumidores posteriores no releen Cosmos para recuperar su configuración B.2.

## CURRENT: piezas de Adoption existentes en código

En `scopes/ada-command-center/backend/processes/alarms-runtime/` existen `session.py` (`AlarmEvaluatorRegistry`, `AlarmExecutionSession`, `build_alarm_execution_session`), `adoption.py` (`AlarmConfigurationRevision`, `plan_configuration_adoption`, dispositions efectivamente declaradas), `adoption_execution.py` y `job_composition.py`, además de composición/durabilidad operacional del Engine. **No afirmar que falta todo Adoption**, pero tampoco inferir de estos componentes un flujo completo desde artefactos locales ni un Effective Head global implementado.

Dispositions comprobadas en `adoption.py` de este corte:

```text
UNCHANGED
COMPATIBLE
DISABLED
REMOVED
STRUCTURAL_RESET
REJECTED
```

Las dispositions históricas/propuestas `ADDED` y `ENABLED` siguen **OPEN**: no aparecen en ese enum actual. No inventar adaptación silenciosa.

## EFFECTIVE HEAD — PROJECT CONTRACT, NO DECLARAR CURRENT

Contrato conceptual precedente:

```text
AlarmEffectiveConfigurationHead
    resolution_key: AlarmResolutionKey
    effective_at: datetime
    adoption_id: str
```

La adopción es **global**, no existe effective separado por Rule o priority group. `EffectiveHead.resolution_key` obliga a que Runtime, Delivery y futura Management Capture consuman EXACTAMENTE la misma key; no escoger latest READY como sustituto.

La ubicación, la serialización, el owner de publicación del Effective Head en el volumen y el mecanismo de lectura deben concretarse **en el incremento de Runtime Adoption**, después de cerrar el contrato local de Materialization. Los nombres `effective.json`/`ready.json` del esquema ilustrativo anterior son PROPOSED, no archivos CURRENT ni un contrato ya implementado.

## Adoption durability y crash safety — contrato anterior preservado

La migración de hot state continúa perteneciendo al WAL del Engine. Contrato target del commit global:

```text
ConfigurationAdoptionCommit
  adoption_id
  previous_resolution_key: AlarmResolutionKey | None
  target_resolution_key: AlarmResolutionKey
  effective_at
  affected_group_commit_ids
```

Secuencia de persistencia Engine vigente en canonical:

```text
WAL -> DURABLE HEAD -> SNAPSHOTS -> MATERIALIZED HEAD
```

Antes de nueva Runtime execution/Adoption/Management Capture o Live materialization que dependa de estado confirmado: `journal.durable == journal.materialized` tras recuperación. No inventar journal paralelo, priority group sintético ni adaptar la salida Cosmos antigua. La publicación local de Materialization es **otro** tipo de persistencia y no confiere authority de runtime durable por sí sola.

## OPEN exclusivo del futuro incremento de Adoption

- Lector exacto de Runtime artifact local usando manifest, integridad y key compartida; ubicación definitiva condicionada a cierre de Materialization.
- Registry/evaluators reales; construir `AlarmExecutionSession` sin ejecutables serializados dentro de configuración.
- Effective Head global y commit/recovery verificables, incluso delivery-only changes que no mutan hot state.
- Universo `source.defined_alarm_identities UNION target.defined_alarm_identities`; reconciliación de ADDED/ENABLED/structural resets y reappearance hot state.
- Occurrence provenance target `resolution_key_at_start` frente a revisions históricas separadas en código actual; reemplazar limpiamente al abordar el cambio, sin alias legacy permanente.
- Test del flujo real con infraestructura cuando esté disponible.

**No abrir estos trabajos en el siguiente chat:** primero terminar y probar únicamente la publicación local de Materialization. Delivery y Live permanecen en otro incremento posterior.

## Contratos target previos preservados expresamente

- `PlannedAlarm.reappearance_after_seconds: int | None` y `reappearance_special_conditions: tuple[AlarmIdentity,...]` son CURRENT. B.2 convierte `after_minutes M -> M * 60` y `None -> None`.
- Reappearance hot-state reconciliation es **OPEN** en Adoption: un cambio de timer/condiciones sobre un ManagementEffect existente requiere semántica explícita, no resolverlo en Materialization.
- Un cambio sólo de visibility/display/Messages/deactivation/visual targets puede no mutar hot state y, aun así, debe avanzar el Effective Head **mediante Adoption durable**.
- Futura Adoption debe contemplar universo definido de origen y destino, y distinguir las Rule intencionalmente eliminadas de inválidas o deshabilitadas.
- El manifest READY local debe permanecer inmutable tras su publicación; el Effective Head nunca puede ser construido como `latest READY`.
