# Alarm Engine — Persistence and Recovery

Estado: **CURRENT — WAL, adopción V1/V2, EngineCommitRecord V3, rebase y recovery durables; qualification física del nuevo proceso UNVERIFIED**. Baseline: `atlanticus@758249d5fa35236b0ac9b990a393083b4463a507`.

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

## CURRENT/FACTS — no confundir generaciones

En la implementación **histórica** de Command Center, `Runtime CURRENT v1` y `Runtime FACTS v2` fueron copias derivadas de commits durables, con cursores de exportación/consumo independientes. Los campos `previous_batch`, los controles de continuidad y el comportamiento fail-closed formaban parte de ese pipeline histórico.

El proceso actual `scopes/ada-alarm-engine/processes/alarm-runtime` compone recovery, evaluación, lifecycle, commits y adopción, pero **no incluye en esa composición el exportador CURRENT/FACTS histórico**. Por lo tanto, su equivalencia de publicación y el consumo físico downstream son **UNVERIFIED**, no parte del cierre de 13F.2c.

Cualquier evolución de exportación debe preservar que los outputs son derivados del WAL; no introducir otro writer de verdad operacional. Volúmenes históricos FACTS v1/v2 requieren inventario/qualification específicos antes de reutilizarlos.

## Evidencia y límites

**VERIFIED por revisión de código:** WAL V1/V2, group commit V3, rebase, adopción operacional, recovery exacto y fencing. Las pruebas actuales cubren fallos antes/después de Durable Head, materialización parcial, takeover, idempotencia de recovery, snapshots V3 e integración Runtime con adopción/ciclos.

**VERIFIED local reportado por usuario:** 471 PASS, Ruff PASS, lock PASS (2026-10-08).

**HISTORICAL:** pruebas físicas y exportación CURRENT/FACTS de Command Center. No son qualification del nuevo ejecutable.

**UNVERIFIED:** qualification física del nuevo proceso, despliegue concurrente multi-host, Azure, autenticación productiva, backup/restore físico y equivalencia de publicación downstream.
