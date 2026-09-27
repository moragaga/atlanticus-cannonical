# Alarm Engine — Runtime Adoption and Effective Configuration

Estado: **CURRENT / LECTOR EXACTO Y PLANNING B1 IMPLEMENTADOS; EXECUTION COMPLETA Y GLOBAL EFFECTIVE PLANNED**

Corte: `atlanticus@c8f23d91ae1cb817be55b4b812b22ffca518880e`; baseline canonical revisada `58241ddb6db5adbd2e783c7ec9f456f1bda5a321`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.

## Frontera congelada

```text
READY != EFFECTIVE
AlarmResolutionKey = (alarm_configuration_revision, confirmed_tool_catalog_revision)
AlarmConfigurationArtifactRef = (source_key, result_id, manifest_sha256, resolution_key)
```

Un READY B.2 contiene `RuntimeAlarmConfiguration` y `DeliveryAlarmConfiguration` con idéntica `AlarmResolutionKey`. Dos manifestaciones READY pueden conservar la misma Rn/Cn y ser artefactos distintos porque cambió su evidencia. `result_id` y SHA256 del manifest permiten pinning exacto. Materialization **no** confiere autoridad de Runtime al publicar READY; Delivery futura no debe seleccionar latest READY autónomamente.

## CURRENT: lectura y enlace exacto (Incremento A y B1)

`backend/alarms/materialization/local_reader.py` valida manifest, procedencia e integridad por SHA256/size de Runtime y Delivery, su Rn/Cn compartida, status READY y puntero READY si se usa. Falla cerrado ante corrupción sin fallback. El `ready.json` de Materialization es físicamente CURRENT; `effective.json` es sólo un nombre ilustrativo anterior, **no** un archivo existente/contratado.

En Runtime, `RuntimeLocalConfigurationReader` ofrece `load_ready_candidate()` y `load_exact_candidate(result_id,manifest_sha256)` sobre `VOLUMEN_PATH`. `build_alarm_configuration_revision(candidate,evaluator_registry)` construye la sesión con el registro de evaluadores explícito, crea un `AlarmConfigurationArtifactRef` y produce `AlarmConfigurationRevision(artifact_ref,defined_alarm_identities,session)`; el builder asume una candidata previamente verificada por el lector y comprueba coherencia de identidad/key declaradas. No afirma verificar por sí mismo la integridad física de disco ni producir automáticamente qualifications GREEN.

El paquete compartido de Materialization sigue ofreciendo un resolver B.2 **puro**, pero ahora incluye componentes de lectura con I/O; no etiquetar el paquete completo como puro.

## CURRENT: planificador B1

`plan_configuration_adoption(source,target)` exige mismo `source_key`, artefactos distintos, rechaza conflicto para un mismo `result_id` y permite distinta materialización con idéntica Rn/Cn. Compara:

```text
universe = source.defined_alarm_identities UNION target.defined_alarm_identities
```

Disposiciones presentes:

```text
UNCHANGED
COMPATIBLE
ADDED
ENABLED
DISABLED
REMOVED
STRUCTURAL_RESET
REJECTED
```

`ADDED`: identidad ausente de source definido y presente en target (activa o deshabilitada). `ENABLED`: definida/deshabilitada en source y ejecutable en target. `DISABLED`: ejecutable en source, definida/no ejecutable en target. `REMOVED`: definida en source y ausente en target, incluso si antes estaba deshabilitada. `UNCHANGED` puede abarcar Rules definidas y deshabilitadas en ambos extremos; cambios solamente Delivery pueden conservar semántica Runtime igual. `STRUCTURAL_RESET` continúa para cambios de criticality; rechazo conservado para cambios de priority group, kind, evaluator y determinadas mutaciones de routing C1/C3.

`ConfigurationAdoptionPlan.is_adoptable` comprueba ausencia de `REJECTED`, **no** si el ejecutor actual puede aplicar el plan. `requires_execution_upgrade` detecta `ADDED`, `ENABLED` y `REMOVED` de una Rule definida pero no ejecutable en source. El usuario verificó 17 tests de plan y 40 de suite Runtime, Ruff/format/wheel PASS, y Git confirma el código en `c8f23d9`.

## CURRENT pero PARCIAL: ejecución y durabilidad operacional existentes

`session.py` aporta registry, entradas y sesiones; `adoption_execution.py` aporta `AlarmConfigurationAdoptionExecutor` para las transiciones que sabe preparar, delegando commits de grupos en `AlarmRuntimeComposition.commit_batch`. `job_composition.py` exige recovery y `journal.durable == journal.materialized` antes de iterar. `durability.py` usa `AlarmPersistence` y fencing del contexto del job.

**Brecha verificada:** el ejecutor `adoption_execution.py` no fue modificado en B1. Su agrupación accede a `plan.source.plan_for(identity)` para cada cambio no `UNCHANGED`: ese plan falta para `ADDED`, `ENABLED` y la eliminación de una Rule source deshabilitada. Su comprobación inicial de `is_adoptable` no protege contra `requires_execution_upgrade`. El hecho de que el planificador clasifique transiciones nuevas **no** implica ejecución segura, ni reconcilia hot state para ellas.

El esquema vigente `EngineCommitRecord` / `AlarmPersistence.commit_batch` está ligado a priority groups y exige al menos un registro para commit. Cambios sólo de Delivery o de la identidad exacta del artefacto pueden necesitar adopción global sin commit de ningún grupo; hoy no existe un registro durable global de esa decisión. La semántica actual de commit del Engine continúa:

```text
WAL -> DURABLE HEAD -> SNAPSHOTS -> MATERIALIZED HEAD
```

Recovery debe alinear `journal.durable == journal.materialized` antes de operaciones dependientes de estado confirmado. Respetar fencing, diferencias crash before/after durable y fail-closed del Engine; **no** trasplantar mecánicamente el protocolo del publicador local de Materialization.

## PLANNED / NO DECLARAR IMPLEMENTADO: autoridad EFFECTIVE global

Contrato conceptual de la decisión precedente:

```text
AlarmEffectiveConfigurationHead
  resolution_key
  effective_at
  adoption_id
```

**Refinamiento PROPOSED, pendiente de definición durable:** la referencia efectiva también necesita pinning exacto de artefacto B1. El eventual `ConfigurationAdoptionCommit` debería capturar `adoption_id`, referencia anterior opcional, referencia objetivo exacta, `effective_at` y commits de grupos afectados, incluyendo cero grupos si sólo cambian Delivery/metadata. **Estos campos siguen siendo una propuesta de contrato**, no un modelo de persistencia existente ni un formato serializado aprobado. La ubicación, schema, owner, secuencia de publicación y mecanismo de recovery del Effective Head continúan OPEN.

La adopción es global, no un efectivo por Rule ni por priority group. No crear grupo sintético, segundo journal, fallback a latest READY, relectura de Cosmos para reinterpretar B.2 ni puente legacy por comodidad. Un cambio sin mutación hot state debe ser igualmente auditable como adopción si cambia el artefacto efectivo.

## OPEN específicos de futura Adoption

1. Contrato y pruebas de ejecución segura de `ADDED`/`ENABLED` y de `REMOVED` desde source deshabilitada; conservar coherencia de grupos y recuperar autoridad tras crash.
2. Commit global vinculado al artefacto exacto dentro del WAL del Engine, con cero o más group commits, recovery/fencing y publicación EFFECTIVE sin adelantos.
3. Cómo construir bootstrap inicial y recuperar fuente/target efectivo sin asumir que el READY más reciente está adoptado.
4. Política explícita para cambios de evaluator/kind/priority group: decisions B.1 desean `COMPATIBLE` o migración estructural, código actual rechaza. No mutar política accidentalmente.
5. Reconciliación de reappearance timer/condiciones especiales sobre `ManagementEffect` vigente; ocurrencias actuales conservan revisiones separadas en vez del modelo objetivo `resolution_key_at_start`.
6. Productores operacionales de evaluator/Tool GREEN y pruebas reales de infraestructura; el `AlarmEvaluatorRegistry` local no demuestra por sí solo qualification/despliegue real.
7. Integración futura de Delivery local exacto, Live/Management Capture **sólo después** de acordar autoridad EFFECTIVE.

## Único siguiente foco técnico

**PROPOSED: diseño y revisión de la frontera Runtime Adoption durable.** Verificar HEAD y releer `adoption.py`, `adoption_execution.py`, `session.py`, `job_composition.py`, `composition.py`, `durability.py`, `alarms/persistence/{models,store}.py`, contratos Core de reconciliación y los tests B1. Diseñar en ese orden: límite del ejecutor actual y guardado seguro del plan; commit global con identidad exacta sobre WAL ya existente; crash/recovery y Effective Head. Acordar un incremento mínimo verificable **antes** de cualquier edición. Delivery, infraestructura y UX quedan fuera de este foco.
