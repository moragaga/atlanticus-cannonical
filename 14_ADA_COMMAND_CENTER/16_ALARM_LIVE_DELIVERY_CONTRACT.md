# ADA Command Center — Alarm Live Delivery Contract

Estado: **PROJECT CONTRACT AGREED para Live; CURRENT/IMPLEMENTED únicamente para Engine outputs e input receiver; Live Projection aún NOT IMPLEMENTED**. Corte documental: 2026-09-28. Este reemplazo preserva los contratos conceptuales ya acordados y actualiza el estado de B2c.7 sin adelantarlos como servicios productivos completos.

## 1. Authority checkpoint y alcance de verificación

```text
Implementación commit del hito leído directamente en Git:
    moragaga/atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae
Implementación último HEAD remoto leído (cambio posterior sólo ADA Generic):
    moragaga/atlanticus@bc1d73742bcb04eb495bbbb1725a8ad23d4eff38
Decisions leído:
    moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical base de estos reemplazos:
    moragaga/atlanticus-cannonical@5558cf9d92d9b21758500024b6099011416d78da
```

El HEAD local final es evidencia aportada por el usuario, no un commit remoto descargado durante este cierre. B2c.7d obtuvo 32 PASS específicas, 162 PASS/1 SKIPPED de regresión conjunta y Ruff PASS en Python 3.14.2 según logs locales; no es CI ni qualification Docker. Distinciones obligatorias: `VERIFIED / CURRENT`, `PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED`, `OPEN`, `CONFLICT`.

## 2. Propósito y ownership

El contrato separa:

```text
B.2 Delivery Configuration (READY exacto)
+
EngineResolvedCurrentState CURRENT v1 publicado
        |
        v
Delivery input receiver CURRENT
        |
        v
[PLANNED] Live Delivery (materialización/enriquecimiento)
        |
        v
[PLANNED] AlarmLiveProjection
        |
        v
Operational Web / Management Capture (separados)
```

Live Delivery **no** es otro Engine: no descubre Tools, consulta SharePoint, resuelve Message precedence, evalúa Rules, recalcula lifecycle/priority/routing ni lee el WAL como API de operación. El input receiver de B2c.7b/d almacena datos validados y un cursor: no tiene todavía responsabilidad de crear el `AlarmLiveProjection` ni registrar despachos operativos.

## 3. Alignment con EFFECTIVE

Se exige la misma `AlarmResolutionKey` y además el **artefacto exacto**:

```text
EngineResolvedCurrentState.resolution_key
  == DeliveryAlarmConfiguration.resolution_key
  == EFFECTIVE.target_artifact_ref.resolution_key

Exact pin = source_key + result_id + manifest_sha256 + resolution_key
```

No usar latest READY, highest revision, fallback ni reinterpretar datos con otra qualification. El lector de entrada Delivery verifica la proyección `runtime/state/effective-head.json` y `LocalAlarmMaterializationReader.read_exact_ready`; no consulta directamente el WAL. La validación duradera completa de EFFECTIVE corresponde a Persistence/Engine; no atribuirla automáticamente al lector físico de la proyección.

## 4. DeliveryAlarmConfiguration — CURRENT preexistente

```text
DeliveryAlarmConfiguration:
    resolution_key: AlarmResolutionKey
    alarms: tuple[ResolvedDeliveryAlarm, ...]
```

Conserva todas las Rules **definidas**: activas presentes; disabled presentes con `is_active=false`; TRACE_ONLY presentes; removed ausentes. `DISABLED != REMOVED` y `TRACE_ONLY != REMOVED`. Orden de serialización determinista preferido: `AlarmIdentity`.

## 5. ResolvedDeliveryAlarm — contrato estático

```text
ResolvedDeliveryAlarm:
    identity, is_active, visibility_mode
    display_name, title, cause_template
    kind, criticality, business_category, operational_areas, color
    default_deactivation_policy, messages, visual_targets
```

**No** copiar `rule_name`, `is_special_condition`, `evaluator_key`, parámetros, `priority_group`, `priority_order`, reappearance, escalation, `AlarmRouting`, callables/DataRequirements/DataLoadPlan ni hot state. En particular, nunca entregar `priority_order` a Web para recomputar prioridad.

## 6. Messages y deactivation capability

`ResolvedDeactivationPolicy(enabled,max_duration_hours,approval_required)` es política estática ya resuelta en B.2. `ResolvedDeliveryMessage(message_key,display_text,deactivation_policy)` contiene opciones para nuevas acciones: override ausente hereda Rule default; override presente reemplaza completamente. Delivery **no** vuelve a calcular precedencia. Un Message `is_active=false` sigue siendo definición válida, pero no seleccionable para una gestión nueva; `message_keys=()` es válido. La política `hasta fin del turno` no está definida por este contrato y sigue OPEN.

## 7. Visual targets y Tool ownership

Contrato conceptual:

```text
ResolvedVisualTarget:
    tool_key, tool_kind
    component_keys
    subcomponents: (owner_component_key, subcomponent_key)
    process_projection_mode?
```

Tool Configuration conserva estructura/topología y nombres; Delivery no copia `ToolStructure`, Tool display name, source release por Tool, display names de Component/Subcomponent, linked topology ni layout role. Elegibles actuales: PROCESS e INTEGRATED_OPERATIONS; STRATEGIC sin Alarm Projection sigue fuera. La identidad linked es la pareja orientada owner/subcomponent.

## 8. Routing y visual projection separados

`visual_targets` y Runtime `assignments/pending_assignments` son hechos diferentes. No existe decisión que autorice «visual target visible sólo si Tool assigned». Una superficie que necesite esa intersección requiere decisión separada, no una regla implícita en Delivery o Web.

## 9. EngineResolvedCurrentState — CURRENT como salida serializada v1

B2c.7a implementó `alarms/contracts/engine_resolved_current_state.v1.schema.json` y `AlarmCurrentStatePublisher`; la salida física es `runtime/output/current/latest.json`, con `document_type`, `schema_version=1`, `artifact_ref`, `state` y SHA256. `state` contiene `resolution_key`, `as_of` y todas las occurrences abiertas al terminar el ciclo. Por occurrence:

```text
identity, occurrence_id, episode_id, started_at
current evaluation (evidence OR error), priority disposition/blockers
technical_hold?, management_cycle, management_effect?, deactivation_effect?
pending_deactivation_request?, assignments, pending_assignments
```

Es un snapshot **completo y reemplazable**, no un `GroupRuntimeSnapshot` durable, ni una muestra de prueba, ni un conjunto de deltas por prioridad. La ausencia física se distingue de un snapshot válido con `alarms=[]`. La implementación conservó hashes, timestamp UTC y protección frente a retroceso/conflictos de `as_of`.

## 10. Evaluación actual y Evidence

`AlarmOperationalCycleResult` entrega evaluaciones completas y `GroupLifecycleDecision` ya resuelto. `AlarmEvaluation` aporta `EvidenceSnapshot(contract_key,contract_version,payload)` si corresponde, o `EvaluationError` para ERROR. `RuntimeEvaluationState` caliente no contiene por sí solo evidence físico suficiente para Live. Un ACTIVE→ACTIVE sin nuevo commit puede cambiar evidence y actualizar CURRENT; no reconstruirlo desde muestreo de History.

## 11. Orden de publicación

```text
evaluate -> lifecycle -> management/deactivation -> routing -> priority
-> persist required changes -> durable confirmed
-> build/publish CURRENT v1
-> export FACTS v2 desde commits durables
-> Delivery input receiver
-> [PLANNED] Live materialization
```

La publicación CURRENT no debe preceder el commit que necesita. Si no existe mutación durable, aún se permite CURRENT nuevo. El cursor exportador FACTS sólo avanza tras haber escrito cada lote. Una interrupción entre ambos es recuperable por verificación/idempotencia.

## 12. Cause materialization — CONTRACT AGREED / NOT IMPLEMENTED

`cause_template + current EvidenceSnapshot.payload -> cause_text` se resolverá **backend-side** en el futuro Live Delivery. Web no interpola placeholders. `LiveCause.status` será `RESOLVED`, `TECHNICAL_UNAVAILABLE` o `MATERIALIZATION_ERROR`, con `text: str|None`.

Un ERROR/technical hold no reutiliza un valor físico anterior como current. Un fallo sólo de cause **no** oculta una occurrence operacional real: conserva la occurrence si es publicable y emite diagnóstico. Sigue OPEN el schema evaluator-evidence necesario para validar estáticamente todos los placeholders.

## 13. Technical hold

Una occurrence abierta puede estar bajo technical hold durante la gracia prevista por Core. Si es publicable, Live debe incluir `evaluation_status=ERROR`, `technical_hold.started_at/due_at` y `cause.status=TECHNICAL_UNAVAILABLE`. Technical hold no convierte ERROR en INACTIVE ni habilita evidencia falsa.

## 14. Regla de publicación Live por prioridad — CONTRACT AGREED

```text
Publicar IFF occurrence abierta + Rule existe en Delivery exacto
            + visibility_mode == VISIBLE
            + disposition IN {PREDOMINANT, DEACTIVATED}
```

| Disposición del Engine | VISIBLE | TRACE_ONLY |
|---|---|---|
| PREDOMINANT | publicar | omitir |
| DEACTIVATED | publicar | omitir |
| ECLIPSED | omitir | omitir |
| CASCADE_SUPPRESSED | omitir | omitir |

Priority se determina en Engine; TRACE_ONLY predominante **no** promueve otras Rules para Web. Este contrato no implementa la proyección por el solo hecho de que CURRENT ya incluya disposition de todas las abiertas.

## 15. Managed y deactivated en Live

`ManagementEffect` no altera verdad física. Una Rule ACTIVE/PREDOMINANT gestionada y otra ACTIVE/DEACTIVATED pueden ser ambas visibles si cumplen el criterio. `ECLIPSED` y `CASCADE_SUPPRESSED` se excluyen de Live, no de la trazabilidad de Engine. El Web no recalcula predominancia.

## 16. Campos actuales de management/deactivation

Si existen: `CurrentManagementState(effect_id,effective_at,reappearance_due_at?)` y `CurrentDeactivationState(effect_id,effective_from,effective_until)`; `management_cycle` pertenece a occurrence. Políticas estáticas max duration/approval permanecen en Delivery Configuration, no duplicadas como verdad operacional.

## 17. Pending deactivation request

Sólo asociar pending request si `request.alarm_identity == current identity` y `request.source_occurrence_id == current occurrence_id`; payload mínimo `request_id`, `requested_at`, `effective_until`. No adjuntar una request vieja a una occurrence nueva. **OPEN:** cleanup/invalidation autónoma de pending requests stale sin decisión recibida.

## 18. AlarmLiveProjection — PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED

```text
AlarmLiveProjection:
    resolution_key, as_of
    occurrences: tuple[AlarmLiveOccurrence, ...]
```

`AlarmLiveOccurrence` reúne pin/identidad/occurrence/episode, fechas, evaluation status, priority disposition, display_name/title/cause, kind/criticality/business category/areas/color, hold, management, deactivation, pending request, assignments y pending assignments, Messages/policy y visual targets. El Web no recibe `cause_template`, `priority_order`, overrides de Message, Tool Catalog ni AlarmDefinition.

Ni el `LocalAlarmDeliveryReceiver` ni sus snapshots de inbox son este modelo. No nombrar el proceso B2c.7b como «Live Delivery completado».

## 19. Semántica de snapshot y errores

Una imagen Live representa exactamente `resolution_key + as_of`, es completa y permite `occurrences=[]` válido. Si una occurrence publicable no tiene Rule correspondiente en el Delivery Configuration exacto, fallar con diagnóstico y no descartarla silenciosamente. Un error únicamente de cause se representa como `MATERIALIZATION_ERROR` conservando la occurrence.

## 20. Responsabilidades de Web

Web puede filtrar por active/deactivated/managed y dibujar targets resueltos; **no** ordena priority, promueve ECLIPSED, calcula cascade, resuelve routing o Message overrides ni recalcula capacidad de deactivation.

## 21. Management round-trip — CONTRACT AGREED

```text
ManagementSubmission:
    input_id, resolution_key, alarm_identity
    source_occurrence_id, source_evaluated_at
    tool_key, message_key?, requested_deactivation_until?
```

El browser devuelve **intención**, no evidence/política como autoridad. `source_occurrence_id` es el objetivo operacional; `source_evaluated_at` es provenance auditora, **no** optimistic lock. Management Capture valida pin EFFECTIVE exacto, Message/capability y agrega actor/timestamp confiables. Este servicio queda fuera de B2c.7.

## 22. Engine mantiene autoridad de Management

Target Engine input ya acordado:

```text
ManagementAction:
    input_id, alarm_identity, source_occurrence_id
    tool_key, actor_key, source_created_at
    deactivation_intent? (effective_until, approval_required)
```

El Engine decide `EFFECTIVE`/`ADDITIONAL`/`LATE` incluso si Capture validó una intención que ya envejeció. CURRENT/Core contienen modelos de effect, pending request, Journey e InputReceipt. Eso no acredita que Management Capture/Web estén implementados.

## 23. CURRENT disponible y pendiente real

**VERIFIED en código/gates locales de B2c.7:** `AlarmOperationalCycleResult` conserva evaluaciones; Engine publica CURRENT serializado v1 de las occurrences abiertas; exporta FACTS v2 confirmados e inmutables; Delivery input receiver valida/copía ambas salidas y conserva cursor independiente; test de integración controlada cubrió datos NOTPII y recreación de instancias.

**NOT YET IMPLEMENTED:** enriquecimiento Live definitivo, `AlarmLiveProjection`, materialización de cause, registro de despachos/escalamientos propios de Delivery, History/Analytics, Web y Management Capture E2E.

## 24. OPEN de diseño/qualification

- Owner/package concreto de la fase **Live materializer**, aunque el package actual `processes/alarms-delivery` ya es owner del **input receiver**. No crear otro proceso remoto por inferencia.
- Schema evaluator-evidence para validación de `cause_template`.
- Proveedor concreto y semántica de `shift_end`, no inventada.
- Ciclo de vida de pending requests stale.
- Store/schema/version/codec finales del **snapshot Live** (distintos de CURRENT input v1 ya implementado).
- Cadencia de publicación Live y retención histórica/datos externos.
- Eventual intersección visual-target/routing, sólo si producto la decide.
- Qualification distribuida, Docker independiente y uso físico/multi-host de Engine/Delivery.
- Migración controlada si existen cursores/lotes FACTS v1; no añadir runtime adapter legacy.

## 25. Conflictos con decisions que no resuelve este cierre

B.1 frozen Special Cascade y suppression uniforme por ranking CURRENT requieren reconciliación formal. La regla de Message inactivo válido/no seleccionable para nuevas acciones también debe reconciliar ciertas formulaciones históricas de B.1/B.2. `adoption.py` conserva rechazos frente a compatibilidad/migración esperada de cambios evaluator/kind/group. Documentar diferencias sin editar código en este cierre.

## 26. Invariantes congelados

```text
Runtime/Delivery config comparten AlarmResolutionKey y exact artifact pin.
READY != EFFECTIVE; sin fallback a latest READY.
Delivery Configuration incluye defined Rules disabled y TRACE_ONLY; removed ausente.
Delivery no contiene priority source data ni evaluator code.
Message precedence se resuelve una sola vez en B.2.
Inactive Message válido pero no elegible para nuevas gestiones.
ToolStructure no se copia en Delivery; visual targets != routing assignments.
Engine CURRENT usa current-cycle evaluation, no Evidence History.
Web no interpreta cause_template ni recalcula priority/routing/capability.
ERROR técnico no convierte muestras viejas en current evidence.
Cause error no oculta la alarma; fallo por Rule exacta faltante bloquea materialización.
VISIBLE+PREDOMINANT y VISIBLE+DEACTIVATED se publican en Live futuro;
ECLIPSED/CASCADE_SUPPRESSED y TRACE_ONLY no se publican.
Live snapshot es completo para resolution_key+as_of y admite vacío válido.
Management devuelve identidad/intención; Engine conserva autoridad del resultado.
source_occurrence_id es target; source_evaluated_at es provenance, no lock.
B2c.7 FACTS runtime v2 encadena previous_batch; no v1 adapters.
Input receiver no constituye Live Projection ni Analytics implementado.
```

## 27. Siguiente frontera acordada

**PLANNED / foco único siguiente:** qualification de **artefactos distribuidos y ejecución Engine + Delivery en Docker como procesos independientes**. Auditar primero el empaquetado/entrypoints, las dependencias, los schemas existentes, el montaje compartido y el entorno; no inventar nuevos contratos ni implementar Live/History durante este gate. La migración de histórico FACTS v1 requiere decisión explícita si se encuentra un volumen real afectado.
