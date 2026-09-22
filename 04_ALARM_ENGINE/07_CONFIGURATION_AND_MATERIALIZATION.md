# Alarm Engine — Configuration and Materialization

Estado: **B.2 IN PROGRESS / RESOLUTION + RUNTIME ARTIFACT + ROUTING CONTRACT AGREED / NOT YET IMPLEMENTED**

## Authority checkpoint

Implementación auditada:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Canonical base de este delta:

```text
moragaga/atlanticus-cannonical:main
85a12f5ccdf0b5d03992396b13d4b1914b7062a3
```

Decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Este documento distingue explícitamente:

```text
CURRENT / VERIFIED
DECISION RECORDED
PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED
OPEN
```

## Invariantes centrales

DECISION RECORDED:

```text
LATEST SAVED = LATEST VALID_AT_SAVE
```

Una revisión persistida pasó full PRE-SAVE validation en el instante de persistencia.

Eso no implica que permanezca materializable para siempre:

```text
VALID_AT_SAVE
!=
READY_AT_ANY_LATER_TIME
!=
EFFECTIVE
```

Una dependencia externa puede derivar después del save. En ese caso la revisión histórica sigue siendo legítima, pero Materialization puede quedar `BLOCKED` y la revisión EFFECTIVE anterior se conserva.

Otro invariante central:

```text
INVALID != REMOVED
DISABLED != INVALID
READY != EFFECTIVE
```

B.2 nunca elimina silenciosamente una Rule rota para producir un artifact parcial.

## CURRENT antes de implementar B.2

Alarm Configuration ya dispone de:
- aggregate durable Rules + Messages;
- validaciones locales/cross-rule implementadas;
- Source/Release;
- base Projection exacta;
- Manager/history;
- structured authoring;
- Tool Catalog V1;
- Alarm Tool Reference read model.

Runtime ya dispone de:
- `PlannedAlarm`;
- `AlarmExecutionSession`;
- `AlarmEvaluatorRegistry`;
- adoption/reconciliation;
- routing C1/C2/C3;
- persistence/recovery y hot runtime snapshots.

No existe todavía un owner concreto que implemente el B.2 acordado en este documento.

## Tres capas de validación

DECISION RECORDED + PROJECT CONTRACT AGREED:

```text
PRE-SAVE VALIDATION
-> antes de persistir Alarm Configuration
-> valida el candidato completo
-> blocking finding impide nueva revisión

MATERIALIZATION VALIDATION / B.2
-> vuelve a validar revisión persistida + dependencias actuales
-> detecta drift posterior al save
-> produce READY o BLOCKED

RUNTIME ADOPTION
-> valida integridad/runtime compatibility
-> aplica transición desde el estado EFFECTIVE actual
-> decide si la revisión puede pasar a EFFECTIVE
```

Regla transversal:

```text
LOCAL VALIDATION
-> tipo, forma, rango e invariantes intrínsecos

B.2 VALIDATION
-> relaciones entre campos
-> relaciones entre Rules
-> referencias externas
-> semántica cross-contract

RUNTIME ADOPTION
-> transición sobre occurrences/episodes/estado durable existente
```

B.2 no corrige silenciosamente configuración inválida; produce findings.

## Inputs de Resolution

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
Published Alarm Configuration Release
+ Confirmed Tool Catalog
+ current Tool reconciliation qualification
+ deployed evaluator catalog/registry for qualification
        |
        v
B.2 Configuration Resolution
```

La adquisición de estos inputs pertenece al Materialization Job.

Runtime no debe convertirse en:
- SharePoint downloader;
- Tool discovery process;
- Message resolver;
- cross-configuration validator.

## Resolution identity / provenance

El `resolution_key` mínimo queda:

```text
AlarmResolutionKey
    alarm_configuration_revision
    confirmed_tool_catalog_revision
```

No se agrega `evaluator_registry_revision` porque CURRENT no existe un contrato de revisión del registry de evaluadores.

La ausencia de esa revisión no autoriza a inventar una provenance ficticia.

La provenance del software desplegado puede seguir identificándose mediante el artifact/runtime version correspondiente, separada de la identidad de configuración.

## Resultado de B.2

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
AlarmConfigurationResolution
    resolution_key
    status: READY | BLOCKED
    findings: tuple[AlarmResolutionFinding, ...]
    runtime_configuration: RuntimeAlarmConfiguration | None
    delivery_configuration: DeliveryAlarmConfiguration | None
```

### READY

```text
READY
<=> no existe BLOCKING finding
<=> runtime_configuration existe
<=> delivery_configuration existe
<=> ambos artifacts comparten exactamente el mismo resolution_key
```

### BLOCKED

```text
BLOCKED
<=> existe al menos un BLOCKING finding
<=> runtime_configuration is None
<=> delivery_configuration is None
<=> EFFECTIVE no avanza
```

No existe publicación parcial por Rule ni artifact parcial por capability.

Runtime y Delivery no son dos verdades independientes.

## Findings

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
AlarmResolutionFinding
    code
    severity: BLOCKING | WARNING
    message
    alarm_identity?
    field_path?
    reference_key?
```

Semántica:
- `code`: contrato estable de máquina;
- `message`: diagnóstico humano;
- `alarm_identity`: Rule afectada cuando aplica;
- `field_path`: ubicación contractual del problema cuando aplica;
- `reference_key`: dependencia involucrada cuando aplica.

### Severidad

```text
BLOCKING
-> cualquier finding que pueda alterar Runtime, adoption, routing,
   management, deactivation o Delivery correcto

WARNING
-> sólo calidad administrativa no contractual
```

Warnings no se usan como escape para permitir contratos rotos.

### Recolección

B.2 debe recolectar tantos findings independientes como pueda determinar con certeza, evitando ruido en cascada.

Ejemplo:

```text
Tool ausente
-> finding tool_not_found
-> no inventar además N component_not_found derivados de esa misma ausencia
```

## Atomicidad de la revisión

Una sola Rule inválida bloquea la resolución completa.

Ejemplo:

```text
A valid
B valid
C evaluator missing
D valid
```

Resultado correcto:

```text
BLOCKED
finding(C, evaluator_not_registered)
no Runtime artifact
no Delivery artifact
EFFECTIVE unchanged
```

Resultado prohibido:

```text
Runtime artifact = A + B + D
```

porque Runtime podría interpretar erróneamente C como `REMOVED`.

## Acquisition failure != B.2 BLOCKED

Si el Materialization Job no puede adquirir de forma confiable los inputs necesarios, no existe todavía un candidato completo resoluble.

Ejemplo:

```text
Confirmed Tool Catalog no pudo cargarse por fallo de infraestructura
```

Esto es:

```text
Materialization Job failure/diagnostic
```

No:

```text
AlarmConfigurationResolution(status=BLOCKED)
```

En ambos casos EFFECTIVE permanece intacto, pero la clasificación causal es distinta.

## Runtime Configuration Artifact

PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED:

```text
RuntimeAlarmConfiguration
    resolution_key
    defined_alarm_identities
    planned_alarms
    parameters_by_alarm
```

El artifact debe ser serializable.

No contiene:
- evaluator callable;
- `DataRequirement` objects como contrato persistido de configuración;
- `DataLoadPlan`;
- `AlarmExecutionSession`;
- lifecycle runtime state.

### defined vs executable

Para una revisión válida:

```text
Rule active
-> identity in defined_alarm_identities
-> PlannedAlarm presente

Rule disabled
-> identity in defined_alarm_identities
-> PlannedAlarm ausente

Rule removed
-> identity ausente de defined_alarm_identities
```

Así Runtime puede distinguir:

```text
DISABLED != REMOVED
```

sin introducir otra representación redundante de `is_active` dentro del artifact.

## Disabled no significa inválido permitido

PROJECT CONTRACT AGREED:

Una Rule `is_active=false` sigue teniendo que ser una Rule contractualmente válida.

B.2 valida también Rules disabled y referencias declaradas en configuración disabled.

Un toggle no puede esconder:
- evaluator inexistente;
- Tool inexistente;
- Message inválido;
- Special Condition inválida;
- routing imposible;
- visual target roto;
- valores contractualmente inválidos.

La diferencia aparece sólo al materializar ejecución:

```text
valid + active
-> executable / PlannedAlarm

valid + disabled
-> defined but not executable
```

## Evaluator boundary

CURRENT:

```text
AlarmEvaluatorRegistry
resolve key = (family_key, evaluator_key)
```

PROJECT CONTRACT AGREED:

B.2 valida que la referencia de evaluator exista para todas las Rules definidas, incluidas disabled.

El artifact conserva la referencia:

```text
evaluator_key
```

pero no el código.

Runtime posteriormente une:

```text
RuntimeAlarmConfiguration
+ deployed AlarmEvaluatorRegistry
-> AlarmExecutionSession
```

Runtime vuelve a resolver el evaluator como defensa en profundidad antes de adoption/execution.

La API concreta del registry para qualification B.2 sigue por implementar; no debe requerir fabricar un `PlannedAlarm` sólo para consultar una key.

## Routing materialization

### Runtime semantics VERIFIED

Engine CURRENT interpreta:
- C1: origin + destinations inmediatos;
- C2: origin inmediato + destinations con `delay_seconds` absolutos desde `occurrence.started_at`;
- C3: origin only.

Management no detiene el reloj de routing.

### Orden de steps

B.2 ordena `AlarmEscalationDefinition.steps` por `step_order`.

`step_order` no necesita ser contiguo.

La posición física dentro de la tuple no define la semántica.

### C1

PROJECT CONTRACT AGREED:

```text
origin
-> inmediato

enabled step
-> RoutingDestination(tool_key, delay_seconds=None)

enabled step + wait_minutes_from_previous_step > 0
-> BLOCKING finding
```

Para C1, `None` o `0` son coherentes con destino inmediato.

Un step disabled no genera destino.

### C2

PROJECT CONTRACT AGREED:

Todos los steps C2, enabled o disabled, requieren:

```text
wait_minutes_from_previous_step is not None
wait_minutes_from_previous_step >= 0
```

B.2 acumula los waits de **todos los steps ordenados**, incluidos los disabled.

Sólo los steps enabled producen `RoutingDestination`.

Ejemplo:

```text
step 1 enabled  wait 15
step 2 disabled wait 20
step 3 enabled  wait 30
```

materializa:

```text
step 1 -> delay_seconds = 15 * 60 = 900
step 3 -> delay_seconds = (15 + 20 + 30) * 60 = 3900
```

El step disabled conserva su intervalo temporal para no adelantar silenciosamente destinos posteriores.

`wait=0` es válido.

### C3

PROJECT CONTRACT AGREED:

```text
origin only
```

Cualquier step `is_enabled=true` produce BLOCKING finding.

Steps disabled pueden permanecer definidos, pero siguen sujetos a validación contractual y de referencias.

### Tool references

Todas las referencias de escalation se validan:
- `origin_tool_key`;
- cada `target_tool_key`, incluidos steps disabled.

Mínimo requerido para una referencia Tool válida:

```text
Tool existe en Confirmed Tool Catalog
AND current reconciliation qualification es GREEN
AND Tool kind es elegible para Alarm Configuration
```

CURRENT Tool Configuration kinds:

```text
PROCESS
INTEGRATED_OPERATIONS
STRATEGIC
```

Strategic no se ofrece en el read model actual de Alarm Tool References y `ToolStructure` no define Alarm Projection para Strategic.

PROJECT CONTRACT AGREED:

```text
STRATEGIC
-> no elegible como Alarm Configuration Tool reference
-> BLOCKING si aparece
```

OPEN:
- restricciones adicionales PROCESS ↔ INTEGRATED_OPERATIONS para origin/destination;
- routing tier matrix.

No inventarlas dentro de B.2 hasta decisión explícita.

## Routing findings mínimos

Códigos conceptuales acordados:

```text
routing_origin_tool_not_found
routing_target_tool_not_found
routing_origin_tool_not_reconciled
routing_target_tool_not_reconciled
routing_tool_kind_unsupported
c1_enabled_step_has_delay
c2_step_wait_required
c3_enabled_step_not_allowed
```

Todos son `BLOCKING`.

Los invariantes locales ya rechazados por `AlarmDefinition` no necesitan ser duplicados mecánicamente por B.2, aunque la frontera debe permanecer fail-closed ante input corrupto.

## Special Condition mapping

B.2 transforma:

```text
AlarmDefinition.reappearance.special_conditions
```

a:

```text
PlannedAlarm.reappearance_special_conditions
```

sólo después de qualification.

Engine no recibe `is_special_condition`.

B.2 debe comprobar que las referencias satisfacen el contrato vigente de Special Condition y scope.

La semántica Runtime de reappearance ya está CLOSED y no se rediseña aquí.

## Visibility conflict — OPEN

Authoring:

```text
visibility_mode=TRACE_ONLY
```

significa:

```text
evaluar + trazar + no publicar visiblemente
```

Engine CURRENT:

```text
delivery_enabled=false
```

produce `SHADOW` y afecta priority/Management.

Por tanto:

```text
TRACE_ONLY != delivery_enabled=false
```

B.2 no debe mapearlos ciegamente.

## Deactivation + Messages — OPEN

No se congela en este delta.

AlarmDefinition conserva:
- `default_deactivation.enabled`;
- `max_duration_hours`;
- `approval_required`.

Messages pueden contener override completo.

Engine `PlannedAlarm.DeactivationPolicy` CURRENT conserva sólo parte de esa semántica.

La distribución correcta entre Runtime Configuration, Management input y Delivery Configuration debe resolverse antes de implementación.

## Provenance naming — OPEN DE IMPLEMENTACIÓN

Runtime CURRENT todavía transporta:

```text
alarm_configuration_revision
tool_registry_revision
```

El contrato B.2 usa:

```text
alarm_configuration_revision
confirmed_tool_catalog_revision
```

La implementación debe reconciliar esta deuda mediante reemplazo limpio del concepto histórico `tool_registry_revision`, sin aliases permanentes ni adapters temporales.

## Runtime Adoption

Materialization responde:

```text
¿el candidato es coherente y puede producir artifacts READY?
```

Runtime Adoption responde:

```text
¿cómo transiciona el estado operacional EFFECTIVE actual a este target válido?
```

Por tanto es legítimo:

```text
B.2 READY
-> Runtime Adoption REJECTED
-> EFFECTIVE permanece anterior
```

CURRENT Adoption:
- criticality mutation -> structural reset;
- C2 routing mutation -> soportada/compatible;
- C1 routing mutation -> rejected;
- C3 routing mutation -> rejected;
- evaluator mutation -> rejected;
- kind mutation -> rejected;
- priority group mutation -> rejected.

B.2 no debe convertir estas limitaciones de transición en invalidez intrínseca del candidato.

## Delivery Configuration Artifact

DECISION RECORDED:

Una resolución READY también produce una Delivery Configuration Projection con el mismo `resolution_key`.

Debe contener configuración ya resuelta suficiente para que Live Delivery no vuelva a SharePoint, Tool Catalog ni catálogos sin resolver.

El schema detallado todavía no se congela en este documento.

Delivery nunca puede adelantarse a Runtime:

```text
READY artifacts
-> Runtime Adoption
-> effective_resolution_key
-> Delivery usa exactamente ese resolution_key
```

## OPEN después de este cierre parcial

Permanece OPEN:
- owner/package concreto de B.2;
- input contractual de current Tool reconciliation GREEN;
- TRACE_ONLY sin abusar de `delivery_enabled`;
- Deactivation + Messages;
- schema completo de Delivery Configuration;
- provenance rename en implementación;
- restricciones adicionales de Tool kinds/tier routing;
- adoption gaps C1/C3/evaluator/kind/priority-group/origin Tool;
- schema/cadence/persistence física del Materialization Job.

## No reabrir Engine

Management suppression y Special Condition Runtime reappearance están CLOSED.

B.2 debe consumir esos contratos; no rediseñarlos por conveniencia de materialización, UI o Delivery.
