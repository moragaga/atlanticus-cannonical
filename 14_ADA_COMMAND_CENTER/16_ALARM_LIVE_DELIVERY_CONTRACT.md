# ADA Command Center — Alarm Live Delivery Contract

Estado: **PROJECT CONTRACT AGREED para Live / NOT IMPLEMENTED; CURRENT/IMPLEMENTED Runtime CURRENT v1 + FACTS v2 y Delivery input CURRENT-only C4, CLOSED en código/regresión local**. Corte documental C4: 2026-09-29. Preserva contratos de Live previamente acordados sin adelantarlos como producción.

## 1. Authority checkpoint y alcance de verificación

```text
Implementación actual inspeccionada directamente:
    moragaga/atlanticus@45eff96d777f4711cb011f779ffc0a6c87bf0ca4
Implementación previa C4:
    moragaga/atlanticus@18029e19ff01e58b9c9399c132ff32b5ca913f06
Decisions HEAD consultado:
    moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical HEAD base de este reemplazo:
    moragaga/atlanticus-cannonical@2e8bbf4780cafc4cea3b18351861aa97a4fb0053
Implementación histórica B2c.7:
    moragaga/atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae
```

B2c.7 histórico reportó 32 PASS específicas, 162 PASS/1 SKIPPED y Ruff PASS en Python 3.14.2 según logs de aquel corte. **C4 nuevo:** 14 archivos modificados solo en Delivery, 29 PASS Delivery, 16 PASS publicadores Runtime, Ruff PASS sobre archivos C4, backend completo **567 PASS/1 SKIPPED** después de `uv sync --locked --all-packages`, con Python 3.14.2, según logs del usuario. No son CI ni qualification Docker. Mantener distinción `VERIFIED / CURRENT`, `CONTRACT AGREED / NOT IMPLEMENTED`, `OPEN` y `CONFLICT`.

## 2. Propósito y ownership

```text
B.2 Delivery Configuration (READY exacto)
+
EngineResolvedCurrentState CURRENT v1 publicado
        |
        v
Delivery input receiver C4 (CURRENT exclusivamente; VALIDATED local)
        |
        v
[PLANNED] Live Delivery (enriquecimiento/materialización)
        |
        v
[PLANNED] AlarmLiveProjection
        |
        v
[PLANNED] Operational Web / Management Capture separados
```

Live Delivery **no** es otro Engine: no descubre Tools, consulta SharePoint, resuelve Message precedence, evalúa Rules, recalcula lifecycle/priority/routing ni lee WAL como API de operación. El receptor C4 valida/almacena CURRENT, **no** crea `AlarmLiveProjection`, no registra despachos operativos ni recibe FACTS. El cursor consumidor FACTS previo es SUPERSEDED para Delivery; Runtime sigue exportando FACTS v2 desde commits durables con su propio cursor.

## 3. Alignment con EFFECTIVE

Se exige `AlarmResolutionKey` igual y el **mismo artefacto exacto**:

```text
EngineResolvedCurrentState.resolution_key
    == DeliveryAlarmConfiguration.resolution_key
    == EFFECTIVE.target_artifact_ref.resolution_key

Exact pin = source_key + result_id + manifest_sha256 + resolution_key
```

No usar latest READY, mayor revisión, fallback ni reinterpretar otra qualification. Delivery lee `runtime/state/effective-head.json` y resuelve `LocalAlarmMaterializationReader.read_exact_ready`; NO lee WAL directamente. La validación durable de EFFECTIVE pertenece a Persistence/Engine, no al lector de la proyección.

**Decisión C4 — desalineación:** cuando CURRENT publicado y EFFECTIVE tienen pines distintos, Delivery devuelve espera y no incorpora el snapshot de la otra configuración. No se agregó coordinación temporal ni sincronización especial: acepta latest CURRENT cuando coincide con EFFECTIVE/READY exacto. Si EFFECTIVE cambia después de que A estuvo staged, los bytes de inbox A pueden permanecer durante espera; el receptor C4 no realiza despacho, y un futuro Live deberá imponer la misma igualdad exacta al usar el inbox.

## 4. DeliveryAlarmConfiguration — CURRENT preexistente

```text
DeliveryAlarmConfiguration:
    resolution_key: AlarmResolutionKey
    alarms: tuple[ResolvedDeliveryAlarm, ...]
```

Conserva todas las Rules definidas: activas presentes, disabled presentes con `is_active=false`, TRACE_ONLY presentes y removed ausentes. `DISABLED != REMOVED`, `TRACE_ONLY != REMOVED`. Orden de serialización determinista preferido: `AlarmIdentity`.

## 5. ResolvedDeliveryAlarm — contrato estático

```text
ResolvedDeliveryAlarm:
    identity, is_active, visibility_mode
    display_name, title, cause_template
    kind, criticality, business_category, operational_areas, color
    default_deactivation_policy, messages, visual_targets
```

NO copiar `rule_name`, `is_special_condition`, `evaluator_key`, parámetros, `priority_group`, `priority_order`, reappearance, escalation, `AlarmRouting`, callables, `DataRequirements`, `DataLoadPlan` ni hot state. No exponer `priority_order` a Web para recomputar prioridad.

## 6. Messages y deactivation capability

`ResolvedDeactivationPolicy(enabled,max_duration_hours,approval_required)` es política estática ya resuelta en B.2. `ResolvedDeliveryMessage(message_key,display_text,deactivation_policy)` contiene opciones de nuevas acciones: override ausente hereda Rule default; override presente reemplaza completamente. Delivery **no** vuelve a calcular precedencia. Message `is_active=false` puede ser definición válida pero no es seleccionable en una gestión nueva; `message_keys=()` es válido. La política `hasta fin del turno` permanece OPEN: no inventar calendario.

## 7. Visual targets y Tool ownership

```text
ResolvedVisualTarget:
    tool_key, tool_kind
    component_keys
    subcomponents: (owner_component_key, subcomponent_key)
    process_projection_mode?
```

Tool Configuration conserva estructura/topología y nombres. Delivery no copia `ToolStructure`, Tool display name, Source Release por Tool, display names de Component/Subcomponent, linked topology ni layout role. Elegibles actuales PROCESS e INTEGRATED_OPERATIONS. STRATEGIC sin Alarm Projection queda fuera de visual targets. Identidad linked: pareja orientada owner/subcomponent.

## 8. Routing y visual projection separados

`visual_targets` y Runtime `assignments/pending_assignments` son hechos diferentes. No hay decisión que exija mostrar un visual target solo cuando la Tool está assigned. Una intersección de ambos requiere decisión futura explícita.

## 9. EngineResolvedCurrentState — CURRENT v1 serializado

`alarms/contracts/engine_resolved_current_state.v1.schema.json` y `AlarmCurrentStatePublisher` producen `runtime/output/current/latest.json`: `document_type`, `schema_version=1`, `artifact_ref`, `state` y SHA256. `state` incluye `resolution_key`, `as_of`, todas las occurrences abiertas al acabar el ciclo. Por occurrence:

```text
identity, occurrence_id, episode_id, started_at
current evaluation (evidence OR error), priority disposition/blockers
technical_hold?, management_cycle, management_effect?, deactivation_effect?
pending_deactivation_request?, assignments, pending_assignments
```

Snapshot completo y reemplazable; no es GroupRuntimeSnapshot durable ni delta por prioridad. Ausencia física no equivale a snapshot válido `alarms=[]`. Hay checksum, timestamp UTC y protecciones frente a regresión/conflicto temporal. C4 consume directamente latest, no reconstruye historia desde FACTS.

## 10. Evaluación actual y Evidence

`AlarmOperationalCycleResult` contiene evaluaciones completas y `GroupLifecycleDecision` resuelto. `AlarmEvaluation` aporta `EvidenceSnapshot(contract_key,contract_version,payload)` cuando procede o `EvaluationError` para ERROR. El estado caliente Runtime por sí solo no posee evidencia física Live suficiente. ACTIVE→ACTIVE sin commit nuevo puede cambiar evidence y generar CURRENT actualizado; no reconstruirlo desde History.

## 11. Orden de publicación

```text
evaluate → lifecycle → management/deactivation → routing → priority
  → persist required changes → durable confirmed
  → build/publish CURRENT v1
  → export FACTS v2 desde commits durables (pipeline productor independiente)
  → Delivery input C4: solo último CURRENT válido
  → [PLANNED] Live materialization
```

CURRENT nunca precede commit requerido; puede publicarse un CURRENT nuevo sin mutación durable. Cursor productor FACTS avanza solo tras lote escrito; interrupciones de export se recuperan mediante verificación/idempotencia. **C4 no requiere ni valida FACTS para recibir CURRENT**.

## 12. Cause materialization — CONTRACT AGREED / NOT IMPLEMENTED

`cause_template + current EvidenceSnapshot.payload -> cause_text` se resolverá backend-side en futuro Live Delivery. Web no interpola placeholders. `LiveCause.status`: `RESOLVED`, `TECHNICAL_UNAVAILABLE` o `MATERIALIZATION_ERROR`; `text: str|None`.

ERROR/technical hold no reutiliza un valor anterior como current. Falla solo de cause NO oculta occurrence real: conservarla si es publicable, emitir diagnóstico. Sigue OPEN esquema evaluator-evidence capaz de validar todos los placeholders estáticamente.

## 13. Technical hold

Una occurrence abierta puede estar bajo technical hold durante gracia de Core. Si cumple filtro Live, incluir `evaluation_status=ERROR`, `technical_hold.started_at/due_at`, `cause.status=TECHNICAL_UNAVAILABLE`. Hold no convierte ERROR a INACTIVE ni habilita evidence falsa.

## 14. Regla de publicación Live por prioridad — CONTRACT AGREED

```text
Publicar IFF occurrence abierta + Rule existe en Delivery exacto
             + visibility_mode == VISIBLE
             + disposition IN {PREDOMINANT, DEACTIVATED}
```

| Disposición Engine | VISIBLE | TRACE_ONLY |
|---|---|---|
| PREDOMINANT | publicar | omitir |
| DEACTIVATED | publicar | omitir |
| ECLIPSED | omitir | omitir |
| CASCADE_SUPPRESSED | omitir | omitir |

La prioridad es autoridad Engine; TRACE_ONLY predominante NO promueve otras Rules para Web. Que CURRENT ya traiga disposiciones no implementa por sí solo el filtro Live.

## 15. Managed y deactivated en Live

ManagementEffect no cambia verdad física. ACTIVE/PREDOMINANT gestionada y ACTIVE/DEACTIVATED pueden resultar ambas visibles si cumplen criterios. ECLIPSED y CASCADE_SUPPRESSED se omiten en Live, no de trazabilidad Engine. Web no recalcula predominancia.

## 16. Campos de management/deactivation actuales

Si existen: `CurrentManagementState(effect_id,effective_at,reappearance_due_at?)`, `CurrentDeactivationState(effect_id,effective_from,effective_until)`; `management_cycle` es de occurrence. Políticas estáticas max duration/approval residen en Delivery Configuration, no duplicadas como estado operacional.

## 17. Pending deactivation request

Asociar pending request solamente cuando `request.alarm_identity == current identity` y `request.source_occurrence_id == current occurrence_id`; payload mínimo `request_id`, `requested_at`, `effective_until`. No anexar request antigua a occurrence nueva. Lifecycle autónomo de pending requests obsoletas sin decisión recibida continúa OPEN.

## 18. AlarmLiveProjection — CONTRACT AGREED / NOT IMPLEMENTED

```text
AlarmLiveProjection:
    resolution_key, as_of
    occurrences: tuple[AlarmLiveOccurrence, ...]
```

Cada `AlarmLiveOccurrence` reúne pin, identidades/occurrence/episode, fechas, evaluation status, priority disposition, display/title/cause, kind/criticality/category/areas/color, hold, management/deactivation, pending request, assignments/pending assignments, Messages/policy y visual targets. Web no recibe `cause_template`, `priority_order`, overrides Message, Tool Catalog ni AlarmDefinition. Ni `LocalAlarmDeliveryReceiver` C4 ni `delivery/input/current/latest.json` constituyen este modelo.

## 19. Snapshot y errores Live

Imagen Live representa exactamente `resolution_key + as_of`, es completa y admite `occurrences=[]`. Si falta Rule exacta en Delivery Configuration para una occurrence publicable, fallar con diagnóstico sin descartarla. Falla solo de cause se materializa como `MATERIALIZATION_ERROR` sin ocultar occurrence.

## 20. Responsabilidades Web

Web puede filtrar active/deactivated/managed y dibujar targets ya resueltos. NO reordena prioridad, promueve ECLIPSED, calcula cascade, reinterpreta routing/Message overrides ni recalcula capacidad de deactivation.

## 21. Management round-trip — CONTRACT AGREED

```text
ManagementSubmission:
    input_id, resolution_key, alarm_identity
    source_occurrence_id, source_evaluated_at
    tool_key, message_key?, requested_deactivation_until?
```

Browser devuelve intención, NO evidence/política autoritativa. `source_occurrence_id` es target operacional, `source_evaluated_at` provenance auditora y no optimistic lock. Management Capture validará EFFECTIVE exacto, Message/capability y adjuntará actor/timestamp confiables. Este servicio permanece NOT IMPLEMENTED.

## 22. Engine mantiene autoridad de Management

```text
ManagementAction:
    input_id, alarm_identity, source_occurrence_id
    tool_key, actor_key, source_created_at
    deactivation_intent? (effective_until, approval_required)
```

Engine decide `EFFECTIVE`/`ADDITIONAL`/`LATE` incluso si Capture validó intención antes de que envejeciera. CURRENT/Core incluyen modelos effect, pending request, Journey e InputReceipt; no demuestran Management Capture/Web E2E.

## 23. CURRENT disponible y pendiente real

**VERIFIED Git C4:** Runtime publica CURRENT v1 y FACTS v2 por separado. Delivery input receiver ahora solo valida/copia último CURRENT, con checks de EFFECTIVE/pin/READY y recuperación del inbox CURRENT; no crea ni avanza cursor de FACTS. Test de integración real en suite Delivery comprueba CURRENT con evidence, reinicio, snapshot válido vacío y permanencia de cadena FACTS **solo en Runtime**. **VERIFIED por logs locales de usuario:** 29 PASS Delivery, 16 PASS publicadores Runtime y 567 PASS/1 SKIPPED regresión backend tras instalar todo workspace; Ruff C4 PASS.

**NOT IMPLEMENTED:** Live definitivo, `AlarmLiveProjection`, cause materialization, despacho/escalamiento operacional propio Delivery, History/Analytics, Operational Web, Management Capture E2E. **UNVERIFIED:** Docker/Azure/CI y montajes físicos.

## 24. OPEN de diseño / qualification

- Owner/package y composición de fase Live materializer; el paquete actual Delivery solo posee input receiver. No crear otro proceso remoto por inferencia.
- Contrato evaluator-evidence capaz de validar estáticamente `cause_template`.
- Proveedor/zona/calendario real para `shift_end`.
- Lifecycle de pending requests obsoletas.
- Schema/version/codec/store finales del snapshot Live, distintos del input CURRENT v1.
- Cadencia de publicación Live, retención histórica y recursos externos.
- Intersección visual-target/routing solo si producto la decide.
- Qualification distribuida de artefactos y ejecución Runtime/Delivery Docker con volumen real; faltan pruebas físicas.
- No hay despliegue previo según usuario: **no inventar** migración FACTS v1 ni compatibilidad temporal.

## 25. Conflictos con Decisions que C4 NO resuelve

B.1 frozen Special Cascade y supresión uniforme por ranking CURRENT precisan reconciliación formal. Message inactivo válido/no seleccionable para nuevas acciones puede contradecir formulaciones históricas B.1/B.2. `adoption.py` conserva rechazos ante compatibilidad/migración esperada por ciertas decisiones de cambios evaluator/kind/group. Registrar esos conflictos sin editar código en C4; documentación Decisions HEAD consultada no tiene nuevo cierre C4.

## 26. Invariantes congelados

```text
Runtime/Delivery config comparten AlarmResolutionKey y pin exacto.
READY != EFFECTIVE; ningún fallback a latest READY.
Delivery input C4 consume únicamente latest CURRENT validado; no FACTS ni cursor receptor.
Mismatch CURRENT vs EFFECTIVE => WAITING_CURRENT; no incorporar ni despachar.
El inbox anterior puede persistir durante espera: el futuro Live deberá validar al leer.
Runtime FACTS v2/cursor productor/WAL siguen intactos.
Delivery Configuration incluye Rules defined disabled/TRACE_ONLY; removed ausentes.
Delivery no contiene priority source data ni evaluator code.
Message precedence se resuelve una sola vez en B.2.
Inactive Message válido pero no elegible para gestión nueva.
ToolStructure no se copia en Delivery; visual targets != routing assignments.
Engine CURRENT usa current-cycle evaluation, no Evidence History.
Web no interpreta cause_template ni recalcula priority/routing/capability.
ERROR técnico no convierte muestras viejas a current evidence.
Cause error no oculta alarma; Rule exacta faltante bloquea materialización.
VISIBLE+PREDOMINANT y VISIBLE+DEACTIVATED serán publicables en Live;
ECLIPSED/CASCADE_SUPPRESSED y TRACE_ONLY no serán publicables.
Live snapshot completo resolution_key+as_of admite vacío válido.
Management devuelve identidad/intención; Engine conserva autoridad resultado.
source_occurrence_id target; source_evaluated_at provenance, no lock.
Input receiver no constituye Live Projection ni Analytics implementado.
```

## 27. Siguiente frontera propuesta

**PLANNED, no iniciada:** qualification de **artefactos Runtime y Delivery en Docker como procesos independientes**, con instalación/entrypoints, volumen físico compartido y pares READY/EFFECTIVE/CURRENT exactos. Auditar primero código y packaging actuales antes de proponer cambios. No mezclar C3/C5, Live/Management/History ni introducir legacy.
