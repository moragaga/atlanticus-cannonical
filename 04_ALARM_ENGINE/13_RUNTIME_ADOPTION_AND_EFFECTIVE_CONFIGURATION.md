# Alarm Engine — Runtime Adoption and Effective Configuration

Estado: **CURRENT — B1/B2a/B2b, composición B2c y publicación B2c.7a/d; recepción Delivery B2c.7b/d; gates locales CLOSED, distribución PLANNED**. Corte 2026-09-28. Commit de alarma verificado remotamente `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`; `main@bc1d73742bcb04eb495bbbb1725a8ad23d4eff38` añadió sólo cambios de ADA Generic. Decisions `50c2bb3...`. Este documento no certifica despliegue físico.

## 1. Invariantes FROZEN

```text
READY != EFFECTIVE
AlarmResolutionKey = (alarm_configuration_revision, confirmed_tool_catalog_revision)
Exact artifact ref = (source_key, result_id, manifest_sha256, resolution_key)
WAL -> Durable Head -> group snapshots -> Materialized Head -> EFFECTIVE projection
Delivery reads only exact EFFECTIVE artifact and Engine outputs
```

Materialization crea pareja Runtime/Delivery READY o BLOCKED, no adopta por sí sola. El WAL del Engine confiere autoridad al pin EFFECTIVE; `effective-head.json` es proyección derivada y recuperable. Rn/Cn sin source/result/hash no identifican la qualification/materialización exacta. No agregar journal paralelo, grupo artificial o adaptador legacy.

## 2. B1 — candidato exacto y planning CURRENT

`backend/alarms/materialization/local_reader.py` valida manifest, identidad, provenance, hashes y parejas Rn/Cn. `RuntimeLocalConfigurationReader` puede leer READY para planificar o exacto para cargar, y `build_alarm_configuration_revision()` exige registry explícito. `plan_configuration_adoption` clasifica por unión de identidades definidas y devuelve UNCHANGED/COMPATIBLE/ADDED/ENABLED/DISABLED/REMOVED/STRUCTURAL_RESET/REJECTED. El planner no convierte una versión inválida en removals. Mutaciones `priority_group`, `kind` y `evaluator_key` continúan rechazadas por la implementación examinada: CONFLICT frente a intención histórica B.1.

## 3. B2a — WAL global V1/V2, ambos CURRENT

`ConfigurationAdoptionRecord` V1 permite adopción global con **cero grupos** (sin fabricar snapshots), incluyendo cambio sólo de contenido de Delivery. `ConfigurationAdoptionRecordV2` permite 1..N grupos con `GroupCommitReference` exactas y último registro de adopción dentro del mismo batch. V1 y V2 del **WAL** NO son las versiones del contrato de lotes FACTS: no borrar V1 por el cambio a FACTS v2. Persistencia confirma durable, materializa y recupera desde WAL sin reevaluar tras crash. Lector externo multiarquivo no debe asumir MVCC físico.

## 4. B2b — EFFECTIVE derivado y exact read

`runtime/state/effective-head.json` contiene `adoption_id`, hash/posición WAL, target artifact ref y `effective_at`. `AlarmPersistence.read_effective_head()` requiere journal alineado, compara proyección con WAL durable y valida snapshots; si hay diferencia exige recovery o falla cerrado. `RuntimeLocalConfigurationReader.load_effective_revision()` reabre el artefacto exacto y crea sesión a partir del registry explícito. `assert_current_effective()` protege la revisión elegida. Nunca utilizar latest READY para simular EFFECTIVE.

## 5. B2c — ejecutable Runtime y fuentes CURRENT

Los puntos de entrada/composición son reales: `processes/alarms-runtime/{application.py,bootstrap.py,process.py,configured_iteration.py,operational_runner.py}`. `build_alarm_runtime_process(...)` recibe por inyección registry, source loader, contratos de evidencia, ID factories y reloj. En el bootstrap operacional existente, el catálogo productivo puede continuar vacío: `catalog/examples/threshold` es una referencia controlada de test, **no** auto-registro real. `AlarmEvaluatorContract` conserva requisitos estáticos o resolver dinámico; cada nueva lógica declara sus requisitos de fuente manualmente. El adapter/reader actual reutiliza fuentes/particiones registradas y entrega datos aislados por alarma; los tests NOTPII no prueban datasets físicos productivos.

La ejecución fijada por job adopta candidato READY según contrato, confirma EFFECTIVE y ejecuta ciclos UTC por segundo con lease/fencing. No redeclarar que todo B2c está sin implementación; tampoco prometer despliegue real.

## 6. B2c.7a — Engine produce CURRENT y FACTS

`AlarmOperationalCycleRunner` verifica la sesión EFFECTIVE fijada, ejecuta `AlarmOperationalCycle` y publica sólo después de los commits necesarios. `AlarmCurrentStatePublisher` entrega CURRENT **v1** completo en `runtime/output/current/latest.json` y puede actualizar evidencia sin nuevo lifecycle commit; un CURRENT ausente no significa imagen vacía. El exportador lee **sólo** commits durables del WAL existente; ahora publica FACTS **v2** en `runtime/output/facts/` y mantiene `runtime/output/state/facts-export-cursor.json`.

El productor incluye `artifact_ref`, hash del commit, posición del journal, colecciones no vacías y `previous_batch` con ID/hash previo o null al comienzo de la cadena inicializada. En una interrupción entre archivo y cursor, relee y revalida en el reintento. Datos anteriores a la inicialización explícita no se exportan fingiendo que nunca existieron; histórico durable existente sin baseline obliga intervención.

## 7. B2c.7b/d — Delivery input receiver CURRENT

`processes/alarms-delivery/{bootstrap.py,settings.py,job.py,receiver.py}` implementa el **receptor de entrada** como otro job con su lease/recovery; **no** es Live Projection ni proceso Engine. Lee la proyección EFFECTIVE publicada (sin leer WAL), comprueba fuente y Materialization exacta, valida checksum/identidad/tiempo CURRENT y recibe lotes FACTS en orden. Conserva `delivery/input/{current/latest.json,facts/*,state/facts-consumption-cursor.json}`.

B2c.7d añade contrato FACTS v2: cada lote identifica al predecesor por ID+SHA; Delivery comprueba continuidad del tramo completo pendiente, la punta del cursor exportador y la cadena histórica recibida durante recovery. Su cursor se actualiza después de guardar cada lote bajo fenced mutation; no altera cursor del productor. CURRENT y FACTS tienen vías/cadencias diferentes y pueden recibirse separadamente.

**Cautela:** el lector EFFECTIVE de este receptor deserializa la proyección física; no afirmar que efectúa por sí mismo toda la validación durable que `AlarmPersistence.read_effective_head()` realiza. Docker y volumen multi-host todavía no se probaron.

**B2c.5c preservado:** `AlarmEvaluatorContract` permite `DataRequirement` estático o `requirements_resolver` (excluyentes), y `DataLoadPlan` consolida vistas sin introducir parámetros web automáticos. En su gate previo se contaron 14 fuentes y 18 combinaciones fuente/partición; FABRICA_KPIS y Meteodata no formaban parte del registro. El código actual no se cambia por este cierre.

## 8. Gates locales y fronteras abiertas

- B2c.7a: 137 PASS/1 SKIPPED; Ruff lint/format PASS, commit local `efe231d...`.
- B2c.7b: 151 PASS/1 SKIPPED; Ruff PASS, commit `94f2621...` inspeccionado remotamente.
- B2c.7c: integración Engine→Delivery 1 PASS; conjunta 152 PASS/1 SKIPPED; commit local `199fc0f...`.
- B2c.7d: 32 específicas PASS; conjunta 162 PASS/1 SKIPPED; Ruff PASS, commit local `c67fcb5...`.

**BLOCKED condicional:** volúmenes/cursor FACTS v1 requieren inventario y decisión explícita para v2; productor/receptor no implementan legacy. **PLANNED próximo foco:** confirmar empaquetado/distribución y Docker de jobs independientes, sin abrir simultáneamente Live, Web, History, cambios de Rules ni adaptación histórica. Meta Python del Project `3.14.7` vs paquetes Command Center `==3.14.2` continúa OPEN separado.
