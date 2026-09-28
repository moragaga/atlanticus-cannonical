# Alarm Engine — Projection and Publication

Estado: **CURRENT — Engine CURRENT v1 y FACTS v2, recepción independiente Delivery; Live/History PLANNED**. Corte 2026-09-28. **Código del hito verificado por lectura remota en** `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`; `main@bc1d73742bcb04eb495bbbb1725a8ad23d4eff38` está un commit posterior con cambios sólo de ADA Generic Master Projection, fuera de este alcance. Los gates locales son evidencia del usuario, no CI de este checkout.

## 1. Fronteras y responsabilidades

```text
Source v3 / ToolDependencyManifest -> B.2 Materialization READY/BLOCKED
                                          |
                                          v
                   Runtime adopta artefacto EXACTO -> EFFECTIVE
                                          |
                     evaluaciones + commits Engine (WAL autoritativo)
                                          |
                           después de la confirmación durable
                          /                               \
               CURRENT v1                              FACTS v2
             snapshot completo                    lotes inmutables encadenados
                          \                               /
                           \-> Delivery input receiver <-/
                                      |
                              inbox + cursor propio
                                      |
                       [PLANNED] AlarmLiveProjection
                       [SEPARATE] History / Analytics
```

B.2 mantiene pareja Runtime/Delivery inmutable READY en `VOLUMEN_PATH/ada-command-center/alarms/materialization/versions/<result_id>/`; `ready.json` sólo señala candidato. BLOCKED no reemplaza READY. B1 selecciona referencia exacta; el WAL del Engine confirma adopción y `runtime/state/effective-head.json` es proyección recuperable. El Engine no debe importar Web/Analytics ni utilizar Cosmos como salida de Materialization obsoleta.

## 2. Identidad exacta congelada

```text
AlarmResolutionKey = (alarm_configuration_revision, confirmed_tool_catalog_revision)
Artifact pin = (source_key, result_id, manifest_sha256, resolution_key)
READY != EFFECTIVE
```

CURRENT, FACTS y Delivery Configuration deben corresponder al artefacto **exacto**, no sólo a las revisiones Rn/Cn. Delivery no toma `ready.json` ni una versión mayor como sustituto. El consumidor de entrada valida el documento EFFECTIVE proyectado y lee READY exacto con `LocalAlarmMaterializationReader.read_exact_ready`; **no** abre el WAL para volver a resolver EFFECTIVE. La verificación estructural de la proyección en Delivery no debe confundirse con `AlarmPersistence.read_effective_head()` y su validación durable completa.

## 3. Publicación Engine: rutas productivas

Archivos del repositorio:

```text
alarms/contracts/engine_resolved_current_state.v1.schema.json
alarms/contracts/engine_committed_facts_batch.v1.schema.json  (histórico, NO runtime compatible)
alarms/contracts/engine_committed_facts_batch.v2.schema.json  (CURRENT runtime)
processes/alarms-runtime/src/.../publication/output_current.py
processes/alarms-runtime/src/.../publication/output_batches.py
processes/alarms-runtime/commented/.../publication/    (espejo español)
```

Salida física con raíz `VOLUMEN_PATH/ada-command-center/alarms/runtime/output/`:

```text
current/latest.json                 # reemplazable
facts/facts-<hash SHA256>.json     # inmutables
state/facts-export-cursor.json     # avance de exportación del Engine
```

Estos JSON se generan en ejecución; los `.schema.json` son contratos fuente, no muestras sobrescritas por el proceso. Los cambios aplicados en B2c.7d hicieron que el productor estricto de FACTS use `schema_version=2`; la presencia del schema v1 en el repositorio no demuestra compatibilidad operativa con un volumen v1.

## 4. CURRENT v1 — implementación observada

`AlarmCurrentStatePublisher` construye `ada_command_center_engine_resolved_current_state`, `schema_version=1`, con `artifact_ref`, `state` y `sha256` del documento canónico. `state` incluye `resolution_key`, `as_of` y `alarms` completas y ordenadas por identidad. Un array vacío es una salida válida. Cada occurrence abierta incluye identidad, IDs de occurrence/episode, inicio, `evaluation` actual (evidence o error), prioridad resuelta, hold, management/deactivation, pending request y assignments.

**No** reconstruye evidencia actual desde History ni vuelve a calcular prioridad. Puede publicar en un ciclo sin mutación durable; si hubo commit requerido, publica tras la confirmación. Conserva una única imagen vigente, no delta por group. Rechaza retrocesos temporales, conflictos para el mismo `as_of` y corrupción de la imagen previa. El consumidor reconoce explícitamente estado ausente como `WAITING_CURRENT` (distinto de CURRENT vacío).

## 5. FACTS v2 — eventos durables y continuidad

`AlarmCommittedFactsExporter` extrae commits confirmados del WAL mediante `AlarmPersistence.read_durable_records()`, sin repetir evaluaciones INACTIVE que no generaron hechos. Lotes posibles: occurrence/episode, Journey, evidencia, gestión/desactivación, routing/assignment e input receipts cuando existen en los registros confirmados. El payload conserva `commit`, `commit_record_hash` canónico `sha256:<64 hex>`, `journal_position`, `artifact_ref`, `records`, `sha256` y, en v2, `previous_batch`:

```text
previous_batch = null
# o
previous_batch = { batch_id: "facts-<hash>", sha256: "<hash del lote anterior>" }
```

El primer lote de una cadena inicializada lleva `previous_batch=null`; los siguientes enlazan ID + digest del anterior, incluso al cruzar publicaciones/revisiones compatibles con el historial. El archivo es `facts/facts-<digest de commit>.json`, no mutable tras publicación. `state/facts-export-cursor.json` pertenece al **productor**, contiene última posición y batch hash; sólo avanza después de escribir el lote. Si falla la actualización del cursor, se reintenta el lote existente comprobando identidad y contenido.

El checksum/cadena detecta borrado, reordenación, alteración y huecos dentro de la secuencia exportada validada. **No** constituye firma criptográfica/autenticación frente a un atacante capaz de reescribir todos los archivos, ni reconstrucción ilimitada de hechos anteriores al baseline. No afirmar esto como garantía global de retención externa.

`initialize_if_needed` falla cerrado ante histórico confirmado sin baseline o archivos FACTS preexistentes no inicializados. Un cursor/volumen v1 requiere intervención/migración explícita: el productor v2 y el receptor v2 no incluyen lector legacy.

## 6. Delivery input receiver — CURRENT

Proceso independiente `processes/alarms-delivery` con entrypoint, configuración, job/recovery y lector de archivos. Mantiene:

```text
VOLUMEN_PATH/ada-command-center/alarms/delivery/input/
  current/latest.json                 # copia validada del último CURRENT
  facts/facts-<hash>.json             # lotes recibidos
  state/facts-consumption-cursor.json # avance independiente de Delivery
```

Validaciones: identidad exacta, checksum, correspondencia de commit/hash/posición, orden temporal, continuidad `previous_batch`, coincidencia con el cursor del exportador y existencia de predecesores, incluido recovery de la cadena histórica recibida. Un lote copiado antes de avanzar el cursor se puede reintentar sin duplicación; Delivery no escribe el cursor del Engine. Con `max_facts_per_iteration`, el receptor verifica la cadena completa pendiente antes de aceptar el tramo correspondiente; un lote intermedio ausente bloquea el avance. CURRENT puede llegar o faltar independientemente de FACTS, sin inventar un snapshot vacío.

El receptor lee la **proyección** física EFFECTIVE; la prueba de integración la alimenta desde el Engine real. Su consumo no crea aún proyecciones Live/History ni hechos de publicación/escalamiento propios de un futuro Delivery operacional.

## 7. Evidencia, límites y siguiente frontera

B2c.7c: integración local controlada Engine real con una alarma, CURRENT/FACTS y recreación independiente de instancias (1 PASS específica; regresión 152 PASS/1 SKIPPED). B2c.7d: 32 pruebas específicas PASS; regresión Engine + Delivery 162 PASS/1 SKIPPED; Ruff lint PASS y 61 archivos formateados. Estos resultados pertenecen al entorno local del usuario Python 3.14.2.

**UNVERIFIED / PLANNED:** build y distribución reales, prueba de procesos Docker separados, reinicios físicos bajo lease, volumen multi-host, CI limpia, volumen histórico v1 y autenticidad/retención de almacenamiento externo. Antes de Live, el siguiente foco acordado es **qualification de distribución + Docker Engine/Delivery**. Management Capture, visualización y Analytics son fronteras separadas.
