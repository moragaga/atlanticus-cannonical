# Alarm Engine — Persistence and Recovery

Estado: **CURRENT / IMPLEMENTED / GATES LOCALES CLOSED; INFRAESTRUCTURA FÍSICA UNVERIFIED**.

Implementación: `scopes/ada-command-center/backend/alarms/persistence`, verificada en `atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`.

## Frontera durable existente

```text
Authority check -> validar/alinear JournalHead y previous state
-> fenced append + flush/fsync WAL -> JournalHead.durable
-> materializar snapshots -> JournalHead.materialized
-> (si hay adopción) proyectar runtime/state/effective-head.json
```

El WAL es **la única autoridad**; `JournalHead.durable` confirma exactamente hasta qué byte y registro existe autoridad durable. `materialized` indica cuánto replay/snapshot está materializado. El Effective Head es una **proyección reconstruible**, no journal adicional ni sustituto del WAL. Ni el READY de Materialization ni los snapshots de grupo confieren autoridad global.

## Contratos de adopción CURRENT

**B2a.1 — `ConfigurationAdoptionRecord` v1:** registro de adopción global durable de un artefacto exacto sin group commits. Incluye `adoption_id`, `previous_artifact_ref` opcional, `target_artifact_ref`, `effective_at`, `committed_at` y hash canónico. Permite primer bootstrap sin inventar grupo ni snapshot; se conserva V1 como formato persistido legítimo.

**B2a.2 — `ConfigurationAdoptionRecordV2` v2:** liga 1..N `EngineCommitRecord` mediante referencias exactas ordenadas `(priority_group, commit_id, record_hash)` y un registro final de adopción dentro de un único batch WAL. Exige referencias, revisiones Rn/Cn de los grupos y hora UTC compatibles con su target. Los commits de grupo convencionales conservan comportamiento independiente. V2 no reemplaza V1 ni crea un decodificador legacy transitorio.

El validador de durable distingue V1/V2, comprueba cadena de adopciones global y cadenas por grupo, detecta referencias/hashes incorrectos e impide absorber silenciosamente histórico legado de grupos sin primera migración explícita. No se implementó esa migración. Los registros ordinarios de grupo pueden coexistir con adoptions legítimas.

## B2b.1 — Effective Head CURRENT

Ubicación:

```text
VOLUMEN_PATH/ada-command-center/alarms/runtime/state/
  journal-head.json
  effective-head.json
  groups/<priority_group>.json
```

Contrato `alarm-effective-head.v1`, `AlarmEffectiveConfigurationHead`:

```text
adoption_id
adoption_record_hash
adoption_position: JournalPosition
target_artifact_ref: AlarmArtifactRefSnapshot
effective_at
```

`target_artifact_ref` identifica `source_key`, `result_id`, `manifest_sha256`, `alarm_configuration_revision` y `confirmed_tool_catalog_revision`. El documento se obtiene de la **última adopción durable** después de completar la materialización; nunca se toma de `ready.json` ni de la última Rn/Cn por aproximación.

`AlarmPersistence.read_effective_head()` exige `durable == materialized`, valida la región durable, compara el Effective Head con el esperado desde el WAL y verifica los snapshots durables de grupos. Sin adopción durable legítima puede devolver `None` cuando no existe proyección EFFECTIVE. Una proyección faltante, dañada, obsoleta o una desalineación exige recovery, sin reparación silenciosa en el lector. Una proyección huérfana sin autoridad durable falla cerrada; tampoco se promueve a autoridad por existir en disco.

## Recovery CURRENT

- **Antes de durable:** eliminar tail WAL no confirmado, sin inventar adopción.
- **Después de durable:** replay exacto desde WAL, sin volver a evaluar Rules.
- **V2:** materializar todo su batch asociado sin avanzar el cursor materialized entre grupos; retry idempotente de snapshots ya escritos.
- **Tras materialized y antes de EFFECTIVE:** recovery recompone la proyección desde el WAL, incluso cuando el journal ya está alineado.
- **Al reparar EFFECTIVE:** contrastar snapshots reales con últimas imágenes de grupo confirmadas; corrupción o discordancia impide publicación.
- **Takeover/fencing:** exigir autoridad y `fenced_mutation` por etapa irreversible, como el resto de Persistence.

**Límite relevante para consumidores:** el archivo físico de cada snapshot se reemplaza individualmente. El batch V2 ofrece **confirmación durable y cursor materialized agrupados**, pero un lector externo que lea archivos de grupo directamente durante una materialización puede observar snapshots parciales. Los consumidores futuros deben respetar la frontera validada; B2b no declara atomicidad MVCC universal de archivos sin gate.

## Evidencia y límites

B2a.1, B2a.2 y B2b.1 tienen tests de contrato, regresión/recovery y lint/format/build locales informados por el usuario. El HEAD final contiene esos archivos. No se verificó un CI sobre checkout limpio del HEAD final ni la semántica multi-host/FS real del volumen compartido. La integración del ejecutor operacional con `commit_adoption()` sigue **PLANNED — B2c**, no es una capacidad demostrada por estos tests.
