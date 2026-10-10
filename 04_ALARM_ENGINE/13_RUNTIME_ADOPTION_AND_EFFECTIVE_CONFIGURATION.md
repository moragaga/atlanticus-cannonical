# Alarm Engine — Runtime Adoption and Effective Configuration

Estado: **CURRENT — adopción durable y publicación CURRENT/FACTS v4 implementadas; stress sintético local VERIFIED; qualification productiva UNVERIFIED**. Baseline: `atlanticus@c3b8ed3b8de4bbafdaeeff4410d4daaa20bed1b4` (2026-10-10).

## Invariantes congelados — READY / EFFECTIVE

```text
READY != EFFECTIVE
AlarmResolutionKey = (alarm_configuration_revision, confirmed_tool_catalog_revision)
Exact artifact ref = (source_key, result_id, manifest_sha256, resolution_key)
```

Materialization crea READY. Solo una adopción confirmada en WAL otorga autoridad operacional y permite reconstruir EFFECTIVE. No hay fallback al último READY.

## Bootstrap CURRENT

1. Recuperar/validar WAL antes de iterar.
2. Si no existe EFFECTIVE ni snapshots durables, leer READY publicado como **candidato**.
3. Validar identidad de manifest, resolución, sesión de evaluadores y fuentes requeridas.
4. Confirmar `ConfigurationAdoptionRecord` V1 bajo lease/fencing.
5. Recuperar EFFECTIVE y fijar en memoria sesión/lifecycle confirmados.
6. No ejecutar un ciclo de evaluación en la misma iteración de bootstrap.

Ausencia de READY provoca espera controlada; READY inválido o no ejecutable no crea EFFECTIVE.

## Adopción posterior CURRENT

Para cada READY con `exact artifact ref` diferente:

1. Comparar revisiones y contenido con el EFFECTIVE actualmente fijado; verificar plan de compatibilidad.
2. Recuperar el head durable reciente y comprobar su igualdad con lifecycle en memoria antes de preparar cambios.
3. Si no hay grupos materializados, adoptar mediante V1 cuando el contrato lo permite.
4. Con grupos materializados, preparar V2: un `EngineCommitRecord` por grupo existente, con rebase exclusivo para estado inalterado y commit operacional V3 para transiciones reales.
5. Reconciliar cierres por eliminación/deshabilitación, episode, Management/deactivation y technical incidents retirados conforme al Core.
6. Confirmar **todos los commits de grupo y la adopción** en el mismo batch WAL, bajo fencing.
7. Recuperar EFFECTIVE exacto; actualizar memoria solamente tras verificar identidad y estado confirmados.

La adopción V2 exige referencias `(priority_group, commit_id, record_hash)` canónicas y revisiones de destino coherentes. No usar un rebase para ocultar cambios operacionales ni fabricar eventos para habilitar un grupo vacío.

## Rechazo, recuperación e idempotencia

- Artifact idéntico al EFFECTIVE no vuelve a adoptarse.
- Configuración incompatible no reemplaza EFFECTIVE; se conserva el estado anterior.
- Snapshot/head desfasado provoca fallo antes del commit o exige recovery; no avanzar desde memoria stale.
- Caída antes de Durable Head: WAL no confirma la adopción.
- Caída tras Durable Head: recovery reproduce exactamente commits y adopción, sin reevaluar transiciones.
- Materialización parcial de V2 no adelanta Materialized Head como si fuera una adopción completa.
- EFFECTIVE es derivado del WAL y recuperable; memoria del proceso no constituye autoridad.

## Frontera downstream — no transferir evidencia

El proceso actual de Runtime bajo `ada-alarm-engine` tiene implementadas las fronteras de configuración, lifecycle, persistencia y publicación de **CURRENT durable v1** (`current/durable-latest.json`) y **FACTS stream/cursor v4**. La continuidad local de estas salidas con el WAL quedó verificada en estrés sintético, pero **no hay evidencia de equivalencia de contrato con el CURRENT histórico de Modeler ni qualification end-to-end con Delivery**.

El pipeline histórico de Command Center consumía Runtime CURRENT junto con EFFECTIVE y READY exacto, pasaba por Modeler y después Delivery, que publicaba `alarm-live-projection`. Esa implementación histórica no se transforma automáticamente en estado CURRENT de la nueva ruta.

Cuando la nueva integración downstream sea cualificada, deben conservarse los invariantes: Modeler no recalcula prioridad, Delivery no modela, Web no programa alarmas y todos los consumidores usan el mismo artifact exacto. El consumidor directo Runtime → Delivery queda SUPERSEDED como target frente a Modeler intermedio.

## OPEN separado

- Registro de evaluadores productivos (`build_alarm_evaluator_registry()` todavía define `contracts=()`).
- Qualification física productiva del Runtime nuevo con evaluadores y fuentes reales; integración downstream nueva Runtime → Modeler → Delivery.
- Migración de estado del scheduler Modeler entre artifacts A → B, cuando existan colas/timers durables.
- Políticas de handoff ordered/no-drop si un consumidor futuro requiere todas las transiciones, en lugar de CURRENT latest.

## Evidencia de cierre

**VERIFIED:** commits 13F.2c.3a–13F.2c.3c.3 en `main`, pruebas relevantes de WAL, fencing, rebase y adopción operacional; validación local reportada: 471 PASS, Ruff PASS, lock PASS.

**CLOSED:** auditoría 13F.2c.3d sin nueva brecha reproducible de persistencia/adopción que ameritara pruebas redundantes.

**VERIFIED local (2026-10-10):** 553 PASS en cuatro suites y readjudicación sintética 7/7 sobre evidencia persistida.

**UNVERIFIED:** qualification productiva del nuevo proceso y equivalencia de todos los consumidores downstream.
