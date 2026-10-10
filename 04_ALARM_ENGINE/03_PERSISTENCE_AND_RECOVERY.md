# Alarm Engine — Persistence and Recovery

Estado: **CURRENT — WAL durable, rotation, checkpoint dual y compactación controlada; stress sintético local VERIFIED; qualification productiva UNVERIFIED**. Baseline: `atlanticus@c3b8ed3b8de4bbafdaeeff4410d4daaa20bed1b4` (2026-10-10).

## Autoridad durable única

```text
Authority check / recovery de JournalHead
    ↓
validar cadena WAL, snapshots y previous state
    ↓
fenced WAL append + fsync
    ↓
JournalHead.durable
    ↓
materialización de snapshots de grupos
    ↓
JournalHead.materialized
    ↓
EFFECTIVE derivado cuando hay adopción confirmada
```

El WAL es la única autoridad de commits y adopciones. `effective-head.json` es una proyección reconstruible, no un segundo WAL. READY y los snapshots aislados no confieren autoridad global.

En el módulo actual `AlarmPersistencePaths`, el estado se ubica bajo `application_root/alarms/runtime/state/`:

```text
journal-head.json
effective-head.json
groups/<priority_group>.json
```

Los segmentos WAL se encuentran bajo `application_root/alarms/runtime/journal/{open,sealed}`. No suponer un prefijo rígido de Command Center ni derivar `application_root` de la ruta histórica de otro proceso.

## Adopción y group commits CURRENT

- **V1** `ConfigurationAdoptionRecord`: adopción global sin group commits; permite bootstrap sin snapshots ficticios y cambios sin grupos persistidos.
- **V2** `ConfigurationAdoptionRecordV2`: adopción final con 1..N group commits anteriores, ordenados por grupo y referenciados exactamente por `(priority_group, commit_id, record_hash)`.
- **V3** `EngineCommitRecord`: registra after-image recuperable del lifecycle de un grupo, evaluaciones/transiciones y estado de incidentes según el contrato.
- **Rebase** `group-configuration-rebase.v1`: marcador exclusivo dentro de un `EngineCommitRecord` V3. Cambia `state_basis` del snapshot preservando estado operacional y sin crear occurrences, journeys, evidences ni Management artificiales. Puede representar grupos vacíos.
- **Adopción mixta**: cada grupo recibe un commit operacional real si cambia el lifecycle o un rebase exclusivo si no cambia. Los commits y la adopción V2 forman una transacción lógica confirmada en el mismo batch WAL.

El journal valida hashes, previous head, consistencia de referencias/revisiones, orden y límites de batch. No se infiere una migración automática desde historia group-only sin primera adopción legítima.

## Recovery/fencing CURRENT

- Antes de Durable Head: descartar el tail no confirmado. No adoptar ni publicar EFFECTIVE.
- Después de Durable Head: replay de after-images confirmadas, sin reevaluación de negocio.
- En adopciones V2, Materialized Head no avanza a mitad del batch si una materialización falla; al recuperar se completa el grupo lógico y se omiten snapshots ya aplicados.
- Después de materializar y antes de publicar EFFECTIVE: reconstruir la proyección desde el journal confirmado.
- Lecturas de EFFECTIVE exigen journal alineado y validación de snapshots contra región durable.
- En pérdida de lease/takeover, usar `assert_authority`, `fenced_mutation` y comparación de heads antes de fronteras irreversibles.
- El Runtime actualiza memoria solo tras la confirmación durable; en cambio de artifact, recupera EFFECTIVE exacto antes de fijar la nueva sesión.

La alineación del Materialized Head no equivale a atomicidad MVCC universal para lectores de archivos independientes; los consumidores deben usar su frontera de lectura validada.

## Rotación WAL, recovery checkpoint y compactación — CURRENT

La implementación `operational/journal.py`, `operational/incremental.py` y `operational/recovery_checkpoint.py` mantiene el WAL como autoridad de commits. Segmentos horarios pueden sellarse/rotarse; los checkpoints v1 preservan head alineado, EFFECTIVE, snapshots de grupos, secuencia y digest de anclaje WAL.

Se publican dos generaciones alternas en `runtime/state/recovery-checkpoint-0.json` y `runtime/state/recovery-checkpoint-1.json` bajo la raíz operacional del Alarm Engine. Recovery valida checkpoint y replay del sufijo durable; nunca reevalúa negocios ni inventa commits. La compactación exige checkpoints compatibles y retiene frontera de replay/autoridad; valida head, slots y fencing antes de borrar segmentos elegibles. No confundir compactación WAL con retención de facts.

## CURRENT y FACTS — generaciones distintas

**Nuevo Runtime (`ada-alarm-engine`):** CURRENT durable v1 usa `current/durable-latest.json`, `document_type=ada_alarm_engine_durable_current_state`, referencia exacta EFFECTIVE, posición WAL y snapshots de grupo. FACTS v4 usa stream JSONL particionado por hora en `facts/year=.../month=.../day=.../hour=.../part-....jsonl`, con cursor v4 en `state/facts-export-cursor.json`. Los publicadores validan continuidad, hashes, anchor y fencing; no se permite retroceso del CURRENT a una posición previa.

**HISTORICAL:** Command Center publicaba Runtime CURRENT v1 `ada_command_center_engine_resolved_current_state` y FACTS v2/v3 batch, bajo rutas y esquemas diferentes. No renombrar estos contratos como equivalentes: los consumidores Modeler/Delivery históricos no se consideran integrados ni cualificados frente a la nueva salida sin adaptación formal de frontera/contrato.

**OPEN:** retención y reducción efectiva del footprint de FACTS, migración controlada de volúmenes antiguos si procede, y qualification productiva downstream.

## Evidencia y límites

**VERIFIED por revisión de código:** WAL V1/V2, group commit V3, rebase, adopción operacional, recovery exacto y fencing. Las pruebas actuales cubren fallos antes/después de Durable Head, materialización parcial, takeover, idempotencia de recovery, snapshots V3 e integración Runtime con adopción/ciclos.

**VERIFIED local reportado por usuario:** 471 PASS, Ruff PASS, lock PASS (2026-10-08).

**HISTORICAL:** pruebas físicas y exportación CURRENT/FACTS de Command Center. No son qualification del nuevo ejecutable.

**VERIFIED local reportado (2026-10-10):** 553 PASS en cuatro paquetes; stress sintético de diez minutos con Acceptance Gate 7/7, checkpoints progresivos y compactación/recovery comprobados por readjudicación offline.

**UNVERIFIED:** arranque con evaluador productivo/datos representativos, despliegue concurrente multi-host, Azure, autenticación productiva, backup/restore físico y equivalencia de publicación downstream.
