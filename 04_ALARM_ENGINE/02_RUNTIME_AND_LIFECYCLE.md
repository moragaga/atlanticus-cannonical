# Alarm Engine — Runtime and Lifecycle

Estado: **CURRENT — Runtime durable y publicaciones integradas; stress sintético local VERIFIED; qualification productiva UNVERIFIED**. Baseline: `atlanticus@c3b8ed3b8de4bbafdaeeff4410d4daaa20bed1b4` (2026-10-10).

## Boundary actual

El proceso ejecutable de referencia para este hito es `scopes/ada-alarm-engine/processes/alarm-runtime`, con entrada `ada.processes.alarm_runtime.bootstrap` y composición en `composition.py`.

La composición utiliza `DataInputLoader`, `RoutedDatasetSourceReader`, registro de fuentes Operational Data, `AlarmEvaluationCycle`, `AlarmLifecycleCycle`, `AlarmDurableRecovery`, `AlarmDurableCycleCommitter` y `AlarmDurableAdopter`. La ejecución se delega a `execute_job` con recovery antes de las iteraciones y autoridad de lease/fencing en las mutaciones.

El estado BLOCKED previo de `scopes/ada-command-center/backend/processes/alarms-runtime` por dependencias de Operational Data legacy es **HISTORICAL / SUPERSEDED para esta ruta nueva**, no evidencia de bloqueo del Runtime actual. No restaurar contratos legacy ni incorporar adaptadores de compatibilidad por ese motivo.

## Core y orden lógico CURRENT

`reduce_group_cycle` recibe estado de grupo, `cycle_at`, `PlannedAlarm`, evaluaciones, acciones Management, deactivation y factories/resolvers explícitos. La lógica de grupo no necesita estado global.

```text
1. validar ciclo, configuración y fuentes
2. evaluar alarms según la sesión ejecutable
3. preparar Management y deactivation inputs
4. resolver lifecycle físico/técnico y occurrences
5. finalizar Management/deactivation
6. resolver routing y priority
7. emitir GroupLifecycleDecision
8. preparar commits por grupo
9. confirmar WAL bajo fencing
10. actualizar lifecycle en memoria solamente tras confirmar
```

Estados de priority: `PREDOMINANT`, `ECLIPSED`, `CASCADE_SUPPRESSED`, `DEACTIVATED`. No reintroducir `SHADOW` ni `delivery_enabled` al Core.

## Configuración y adopción

- READY contiene un artifact candidato; EFFECTIVE identifica el artifact exacto confirmado.
- Bootstrap sin EFFECTIVE ni snapshots durables usa adopción V1.
- Adopciones posteriores compatibles usan V1 sin grupos cuando proceda, o V2 con 1..N commits de grupo.
- Un grupo sin transición operacional recibe rebase V3 exclusivo, sin inventar eventos físicos.
- Un grupo con cierres, cambios Management/deactivation o resoluciones de incidentes recibe commit operacional V3 real.
- Los commits de grupo y la adopción V2 se confirman en un mismo batch WAL.
- La iteración que adopta no ejecuta simultáneamente un ciclo de evaluación del nuevo artifact; recovery exacto precede a la actualización de memoria.
- Si la configuración es incompatible o el artifact no es ejecutable, se conserva la autoridad anterior y la ejecución con EFFECTIVE puede continuar.

## Contratos técnicos de recovery

`AlarmPersistence` recupera la región durable, valida journal/snapshots y reconstruye EFFECTIVE. `AlarmDurableRecovery` reabre el READY por `source_key + result_id + manifest_sha256`, valida las revisiones y restaura grupos e incidentes. Antes de adoptar un READY nuevo, el job relee la autoridad durable y compara el lifecycle con su memoria, evitando usar heads obsoletos.

Una caída posterior a Durable Head no autoriza reevaluar una transición confirmada: recovery reconstruye el commit exacto. Una caída anterior a Durable Head no crea autoridad ni una adopción ficticia.

## Publicación y mantenimiento — CURRENT

La composición `AlarmRuntimeComposition` reconcilia publicaciones derivadas del WAL durante recovery y antes de ejecutar iteraciones. Publica CURRENT durable v1 y FACTS stream/cursor v4. El flujo de mantenimiento confirma recovery checkpoints y habilita compactación de segmentos WAL obsoletos bajo fencing. Recovery, output, mantenimiento y cierre conservan la autoridad durable como fuente, no los archivos de salida.

El Runtime genérico incorporó `JobDefinition.iteration_summary_every: int = 1` y `JobRuntimeContext.request_iteration_summary()`. El valor `1` mantiene la política previa en consumidores no modificados; Alarm Runtime establece `0` y solicita resúmenes significativos. Los cambios conciernen logging/observabilidad, sin alterar los contratos de lease/ciclo.

## Alcance y evidencia

**VERIFIED:** código y pruebas de los incrementos 13F.2c.3a–13F.2c.3c.3 publicados; suite local reportada por el usuario: 471 PASS, Ruff PASS, lock PASS.

**VERIFIED local (2026-10-10):** 553 PASS en contratos, runtime genérico, persistence y alarm-runtime/stress; stress sintético de diez minutos readjudicado con Acceptance Gate 7/7. La evidencia de estrés anterior fue readjudicada offline, no corresponde a una nueva ejecución del runner actualizado.

**UNVERIFIED:** ejecución con evaluadores productivos concretos, equivalencia downstream con el Modeler histórico y despliegues multi-host/Azure. El registry productivo de evaluadores retorna actualmente `contracts=()`.

Modeler avanzado, scheduler, Delivery y superficies Web se mantienen fuera de este hito; sus contratos no se infieren de la implementación de Runtime.
