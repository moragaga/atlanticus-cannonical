# Alarm Engine — Configuration and Materialization

Estado: **CURRENT / SOURCE V3 + PURE B.2 + EXECUTABLE JOB v0.2.1 / LOCAL PUBLICATION DECIDED, PLANNED**

Fuentes del corte: `atlanticus@b600ca591b56d0924aed752dfae6e9fab2c6f1d6`, `atlanticus-cannonical@772d15078c97802d58d8b658b0d5d5b928fa2ed5`, `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Este documento separa estado publicado en Git de la decisión posterior aún no integrada.

## Invariantes anteriores congelados

```text
LATEST SAVED = LATEST VALID_AT_SAVE
VALID_AT_SAVE != READY != EFFECTIVE
INVALID != REMOVED
DISABLED != INVALID
DISABLED != REMOVED
TRACE_ONLY != REMOVED
READY != EFFECTIVE
```

B.1 valida guardado/publicación; qualification externa completa corresponde a B.2. El resolver puro, el proceso I/O, los stores de artefactos y Runtime Adoption tienen propietarios diferentes.

## Source y evidencia Tool exacta — VERIFIED / CURRENT

```text
AlarmConfiguration(rules, messages)
AlarmConfigurationSnapshot(configuration, tool_dependencies: ToolDependencyManifest)
source document_type: ada_command_center_alarm_configuration_release
source schema_version: 3
```

V2 está **SUPERSEDED**, sin decoder de compatibilidad. Publicación Rn conserva `ToolDependencyManifest(Cn)` sólo para referencias Tool definidas: origen, todos los escalones habilitados o no, visual targets, reglas activas o inactivas. Cada entrada retiene `display_name`, `source_release_id`, `kind` y `ToolStructure`. `confirmed_tool_catalog_revision` se obtiene de `tool_dependencies.revision`; no consultar latest Tool Catalog para reinterpretar Rn/Cn.

Manager Alarm mantiene la revisión Tool como sidecar del draft y vuelve a comprobar drift en Validate/Publish. Los productores reales de Tool GREEN y Evaluator qualification siguen pendientes de contrastar.

## Proyección operacional — CURRENT ADAPTERS / E2E UNVERIFIED

Existen `web/alarms/configuration/{source_release,source_projection,projection_record}.py`, `web/alarms/projection-local`, `web/alarms/projection-cosmos` y `web/alarms/persistence`. `ProjectionRecord[AlarmConfigurationSnapshot]` conserva release, procedencia y payload íntegro. `ProjectionStore.get_active(source_key)` devuelve la proyección activa, no una API histórica por release. Materialization captura/revalida la release exacta durante su iteración. No inferir que la proyección real se publica en Cosmos: no hay infraestructura ni prueba end-to-end presentada.

## Pure B.2 — VERIFIED / CURRENT

Owner: `scopes/ada-command-center/backend/alarms/materialization`.

```text
AlarmConfiguration + alarm_configuration_revision
+ ToolDependencyManifest(Cn) exacto
+ ToolReconciliationQualification
+ EvaluatorQualificationCatalog
-> resolve_alarm_configuration(...)
-> AlarmConfigurationResolution(READY | BLOCKED)
```

Toda Rule definida requiere evaluator cualificado por `(family_key,evaluator_key)` y todas las referencias Tool definidas deben existir y ser GREEN (también Rules inactivas y steps disabled). Valida routing y visual targets. Hallazgos como `evaluator_not_qualified`, `tool_reference_not_found`, `tool_reference_not_green`, `routing_invalid_for_criticality`, `routing_invalid_direction` y `visual_target_invalid` pertenecen al resultado B.2, no a fallas operacionales de I/O.

```text
READY   -> RuntimeAlarmConfiguration + DeliveryAlarmConfiguration con idéntica AlarmResolutionKey
BLOCKED -> findings bloqueantes + Runtime None + Delivery None
```

El resolver no hace I/O, retry, persistencia, scheduler ni Adoption.

## Strict routing — FROZEN / IMPLEMENTED

`PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END`. Cada step habilitado avanza exactamente un nivel respecto del anterior habilitado, en orden ascendente. Sin mismo nivel, retroceso ni saltos. Strategic es terminal. C1/C2 permiten cero destinos; C3 sólo origen. C1 habilitado inmediato; C2 espera positiva desde el step habilitado anterior y B.2 acumula offsets desde `occurrence.started_at` (20 + 20 => 20 y 40 minutos). Steps disabled no enrutan ni consumen tiempo, pero sus referencias sí se cualifican. La Web comparte la policy; Strategic sólo routing, no visual targets.

## Process publicado en main — VERIFIED / CURRENT DE IMPLEMENTACIÓN

Ruta: `scopes/ada-command-center/backend/processes/alarms-materialization/`; versión **0.2.1** en HEAD. Contiene `__init__.py`, `__main__.py`, `bootstrap.py`, settings, `AlarmCandidateAcquirer`, `AlarmMaterializationCandidate` con representación JSON congelada/fingerprint, proveedor `JsonFileAlarmQualificationProvider`, `AlarmMaterializationJob`, codecs de Runtime/Delivery y composición con `atlanticus.runtime.execute_job`.

La iteración obtiene proyección activa, fija identidad, carga evidencia JSON exacta, invoca B.2, revalida proyección y digest de qualifications antes de publicación bajo fencing y distingue `READY`, `BLOCKED`, `UNCHANGED`. El código actual está acoplado a `CosmosAlarmMaterializationResultStore` **de salida**: documento con `runtime`, `delivery`, `manifest`, `findings`, hashes y codec de lectura. Es realidad implementada, **pero su destino de salida quedó SUPERSEDED** por la decisión del Project.

El proveedor JSON es una modalidad manual/de prueba controlada existente, **no prueba** que existan productores de GREEN/Evaluator integrados. No incorporar datos sintéticos como evidencia operacional real.

## Nueva decisión de arquitectura — DECIDED / PLANNED

```text
Cosmos: ProjectionRecord[AlarmConfigurationSnapshot]
  -> Materialization [único lector Cosmos para esta adquisición]
  -> pure B.2
  -> publicación coherente, versionada y local en VOLUMEN_PATH
  -> Runtime/Engine y Delivery consumen localmente el artefacto EXACTO adoptado
```

Una versión READY contiene ambos contratos con una misma `AlarmResolutionKey` y un manifiesto capaz de comprobar identidad, procedencia e integridad. El mecanismo de publicación debe impedir que Engine o Delivery vean media versión; una falla debe preservar versiones anteriores. Un BLOCKED conserva diagnóstico pero no se publica como ejecutable. No publicar EFFECTIVE desde Materialization; Adoption pertenece a Runtime.

No está congelado todavía: nombre de directorios/archivos, layout multi-instancia, codec reutilizable definitivo, marcadores READY/EFFECTIVE, primitivas atómicas, política de retención y permisos. `runtime.json`, `delivery.json`, `manifest.json` y punteros `ready.json`/`effective.json` fueron **propuestos como ilustración**, no son interfaces vigentes. Contrastar primero `atlanticus.state`, persistencia y capacidades locales existentes. No crear un store genérico o un proceso extra sin justificación.

**No conservar** `CosmosAlarmMaterializationResultStore` como publicación alternativa tras el reemplazo. Sí conservar el adapter Cosmos **de lectura** de la proyección.

## Validación del corte — ACOTADA

Evidencia aportada: `uv sync --python 3.14.2` exitoso; ejecución local sobre la anterior v0.2.0: 11 tests de adquisición fallaron por `tool-a` inválido, cinco findings Ruff de imports y diez archivos pendientes de formato; wheel v0.2.0 creado. El HEAD v0.2.1 contiene fixture corregida `tool_a` y código/formato actualizado. **UNVERIFIED:** rerun de pytest, Ruff, formatter y wheel en un entorno con dependencias reales sobre v0.2.1. **BLOCKED por infraestructura no disponible:** prueba Cosmos/Blob real. No declarar job cerrado E2E.

## Único siguiente foco

Cerrar Materialization **sustituyendo exclusivamente publicación Cosmos por publicación local confiable**, manteniendo adquisición, resolver y modo qualification actual mientras no haya evidencia del productor real. Después ejecutar tests locales/contractuales sin Cosmos real; integración Cosmos se difiere. Runtime Adoption y Delivery NO se implementan en este incremento.
