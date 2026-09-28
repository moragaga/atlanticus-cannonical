# Alarm Engine — Persistence and Recovery

Estado: **CURRENT — WAL, adopción V1/V2, EFFECTIVE derivado y FACTS v2; despliegue físico UNVERIFIED**. Corte 2026-09-28. El exportador no crea otro WAL: consume commits confirmados del existente. **Código del hito verificado por lectura remota en** `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`; `main@bc1d73742bcb04eb495bbbb1725a8ad23d4eff38` está un commit posterior con cambios sólo de ADA Generic Master Projection, fuera de este alcance. Los gates locales son evidencia del usuario, no CI de este checkout.

## Frontera durable única

```text
Authority check -> validar/alinear JournalHead + previous state
-> fenced WAL append + fsync -> JournalHead.durable
-> materializar snapshots -> JournalHead.materialized
-> proyectar EFFECTIVE si hubo adopción confirmada
-> generar CURRENT y exportar FACTS DESPUÉS de confirmación cuando corresponde
```

El WAL es **única autoridad** sobre commits y adopción; Durable Head identifica la región autoritativa y Materialized Head limita replay agrupado. `runtime/state/effective-head.json` es proyección reconstruible, no WAL alternativo. READY y los snapshots por grupo no confieren autoridad global.

## Adopción CURRENT: WAL V1 y V2, ambos válidos

**V1** `ConfigurationAdoptionRecord` admite adopción global sin group commits: incluye ID, referencias source/target, effective/committed timestamps y hash canónico; permite bootstrap sin inventar grupos/snapshots. **V2** `ConfigurationAdoptionRecordV2` liga 1..N group commits ordenados por referencias exactas `(priority_group,commit_id,record_hash)` y un adoption final dentro del mismo batch WAL. Las versiones V1/V2 del **WAL** se conservan: no son equivalentes a los formatos FACTS v1/v2.

El journal y validador comprueban cadenas de adopción y de grupos, hashes, autoridad y previous state. Histórico group-only incompatible sin adopción inicial no se absorbe como autoridad silenciosamente. Materialized Head es barrera agrupada, no atomicidad MVCC universal de múltiples archivos vistos por lectores externos.

## Effective Head y lectura verificada

Ubicación bajo `VOLUMEN_PATH/ada-command-center/alarms/runtime/state/`: `journal-head.json`, `effective-head.json` y `groups/<priority_group>.json`. `AlarmEffectiveConfigurationHead` incorpora `adoption_id`, `adoption_record_hash`, `adoption_position`, `target_artifact_ref` y `effective_at`. `read_effective_head()` exige journal alineado y valida región durable, snapshots y coherencia con última adopción. Sin adopción legítima puede devolver None; cabeza ausente/corrupta/desfasada con WAL durable exige recovery/fail-closed, no latest READY como sustituto.

Runtime reabre artefacto exacto mediante el lector de Materialization y selección EFFECTIVE; su job pinnea sesión. Delivery input receiver B2c.7 lee el documento **proyectado** EFFECTIVE y materialización exacta; no consulta WAL y su `_effective()` no equivale por sí solo al validador profundo `AlarmPersistence.read_effective_head()`.

## Recovery y fencing CURRENT

- Antes de Durable Head: descartar tail no confirmado, sin inventar adoption.
- Después de Durable Head: replay exacto sin reevaluación; V2 agrupa materializaciones antes de avanzar Materialized Head.
- Después de materialized y antes de EFFECTIVE: recomponer la proyección desde WAL; discrepancia de snapshots bloquea.
- En takeover: validar autoridad, comparaciones de heads y `fenced_mutation` en cada frontera irreversible. Nunca asumir single-worker como sustituto de fencing.

## B2c.7 publicación — copias derivadas, no otra autoridad

Engine `runtime/output/current/latest.json` es snapshot v1 reemplazable con SHA256; `runtime/output/facts/facts-*.json` son copias inmutables de eventos de commits durables en formato runtime **v2**, con `previous_batch` ID/hash; `runtime/output/state/facts-export-cursor.json` representa progreso **del exportador**, nunca reemplaza JournalHead ni el WAL. La publicación CURRENT ocurre tras el commit requerido y puede actualizarse si sólo cambia la evidence actual.

Delivery tiene inbox independiente en `alarms/delivery/input` y `state/facts-consumption-cursor.json`. Recibe en orden, verifica continuidad hasta la punta del productor, comprueba la cadena histórica recibida durante recovery y conserva su progreso. Si falta lote inicial/intermedio, la cadena es inconsistente o existen archivos sin cursor de exportación, bloquea: no inventa hechos, no reconstruye un lote perdido y no consume directamente el WAL.

**FACTS v1 runtime → v2 runtime: BLOCKED condicional en volúmenes históricos.** Las versiones del schema v1 pueden permanecer como documento de la genealogía, pero no existe lector/adaptador runtime v1. La generación v2 exige baseline y rechaza archivos/cursor preexistentes incompatibles. Antes de desplegar sobre histórico real, inventariar y acordar procedimiento específico; no borrar data ni crear legacy.

## Evidencia / límites

B2c.7c probó Engine real más Delivery en entorno controlado y recreación de instancias; B2c.7d tuvo 32 tests específicos y 162 PASS/1 SKIPPED conjuntos con Ruff PASS según logs locales. No es prueba multi-host, backup/restore físico, dos contenedores concurrentes, CI limpia ni garantía de autenticidad firmada. La qualification histórica R3.5/F-010 sigue siendo evidencia de su generación anterior, no del contrato runtime FACTS v2.
