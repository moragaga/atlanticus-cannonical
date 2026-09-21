# ADA Command Center — Alarm Configuration Authoring Model

Estado: **DRAFT / AUTHORITY RECONCILED / RECOVERED SEMANTICS CONSOLIDATED / OPEN CONTRACTS REMAIN**

Propósito: **guía canónica de trabajo para diseñar el authoring de Alarm Configuration usando la versión más cercana posible a la autoridad vigente, sin convertir decisiones históricas o recuerdos en contrato actual y sin modificar el Alarm Engine por conveniencia de UI.**

Este documento no congela decisiones nuevas. Consolida:

- implementación CURRENT;
- canonical CURRENT;
- decisiones frozen/recorded aún compatibles;
- qualification histórica;
- conflictos explícitos entre contrato deseado e implementación;
- temas que siguen OPEN.

## 1. Checkpoint de autoridad auditado

### Atlanticus implementado

```text
moragaga/atlanticus:main
762c8db89d8811b2036a84e4c82904e6ee31ec28
```

La implementación en `main` es la realidad ejecutable actual.

### Atlanticus canonical

```text
moragaga/atlanticus-cannonical:main
a91e1d8f176de1616c06f2492154ca9a50cdf0fb
```

Documentos principales contrastados:

```text
04_ALARM_ENGINE/00_INDEX.md
04_ALARM_ENGINE/01_DOMAIN_MODEL.md
04_ALARM_ENGINE/02_RUNTIME_AND_LIFECYCLE.md
04_ALARM_ENGINE/05_PROJECTION_AND_PUBLICATION.md
04_ALARM_ENGINE/06_MANAGEMENT.md
04_ALARM_ENGINE/07_CONFIGURATION_AND_MATERIALIZATION.md
04_ALARM_ENGINE/08_QUALIFICATION_BASELINE.md
04_ALARM_ENGINE/09_DECISION_INDEX.md
04_ALARM_ENGINE/10_OPEN_ITEMS.md
04_ALARM_ENGINE/11_SOURCE_LEDGER.md

14_ADA_COMMAND_CENTER/02_CURRENT_IMPLEMENTATION.md
14_ADA_COMMAND_CENTER/04_CONFIGURATION_SCOPE.md
14_ADA_COMMAND_CENTER/05_TOOL_TO_ALARM_CONFIGURATION.md
14_ADA_COMMAND_CENTER/06_ENGINE_AND_PROJECTIONS.md
14_ADA_COMMAND_CENTER/13_OPEN_ITEMS.md
14_ADA_COMMAND_CENTER/14_TOOL_CATALOG.md
```

### Atlanticus decisions

Fuentes de decisión principales:

```text
alarm_decisions/
R3.6M-006B.1-alarm-definition-contract-inventory-DRAFT.md
R3.6M-006B.1-alarm-definition-contract-inventory-DRAFT_2.md
R3.6M-006B.1-alarm-definition-contract-inventory-DRAFT_3.md
R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md

R3.6M-006B.2-alarm-projection-boundary-DECISION-RECORDED.md
R3.6M-006B.2-alarm-projection-and-publication-boundary-DECISION-RECORDED-INCREMENT-1.md
R3.6M-006B.2-alarm-projection-and-publication-boundary-DECISION-RECORDED-INCREMENT-2.md
```

Qualification y genealogía preservadas:

```text
alarm_test/
R3.5 phases A-F
E-008 .. E-012
F-001
F-002
F-007
F-010
```

El ledger canonical declara F-010 como cierre final `CLOSED PASS/GREEN`.

## 2. Regla de lectura de esta guía

Jerarquía usada:

```text
1. atlanticus:main
2. atlanticus-cannonical:main
3. decisiones FROZEN/RECORDED compatibles
4. qualification / tests
5. drafts históricos
6. memoria conversacional
```

Si una decisión frozen y `main` difieren:

```text
NO se corrige silenciosamente.
NO se declara implementado lo que sólo fue deseado.
Se registra CONFLICT / RECONCILIATION OPEN.
```

## 3. Resultado principal del rastrillo

El modelo de authoring que recordábamos está mayormente respaldado por B.1 y por la implementación de Alarm Configuration.

Sin embargo, el rastrillo encontró una frontera clave:

```text
AlarmDefinition CURRENT
!=
PlannedAlarm CURRENT
```

B.2 sigue sin implementarse.

Por tanto, hoy existen tres capas distintas:

```text
AUTHORING CONTRACT
AlarmDefinition + MessageDefinition
        |
        | CURRENT
        v
Alarm Configuration Source / Projection

RESOLUTION
AlarmDefinition + Tool Catalog + evaluator registry
        |
        | PLANNED / B.2
        v
PlannedAlarm + AlarmExecutionEntry

RUNTIME
PlannedAlarm + AlarmExecutionSession
        |
        | CURRENT / QUALIFIED
        v
Alarm Engine
```

Consecuencia:

> Una capacidad presente en `AlarmDefinition` no implica automáticamente que el Runtime actual ya la consuma.

Esto es especialmente importante para:

- Special Conditions;
- reappearance desde Special Conditions;
- max duration de deactivation;
- Message deactivation overrides;
- visual targets;
- Process projection mode;
- cambios de configuración durante adoption.

## 3.1 Semánticas recuperadas en esta consolidación

Durante el rastrillo de decisions + Engine CURRENT se recuperaron explícitamente estas capacidades:

```text
not_execute
-> corresponde a is_active=false

not_visible
-> corresponde a visibility_mode=TRACE_ONLY

C1
-> origin + todos sus destinos configurados inmediatamente

C2
-> origin inmediato + destinos temporizados

C3
-> sólo origin, nunca escala

Management suppression
-> intención recuperada gobernada por priority_order

Special Condition
-> flag explícito independiente del ranking

Special Condition reappearance
-> trigger OR con timer

GENERIC / DISTRIBUTED
-> variante de presentación por target PROCESS

carousel / queue-in-queue
-> vocabulario visual recuperado todavía no contractual
```

No crear aliases legacy con estos nombres recordados si el contrato CURRENT ya tiene una representación canónica equivalente.

## 4. Agregado durable CURRENT

**VERIFIED / CURRENT**

```text
AlarmConfiguration
├── rules: tuple[AlarmDefinition, ...]
└── messages: tuple[MessageDefinition, ...]
```

Es la unidad editable/publicable.

Tool Catalog no forma parte del payload durable de Alarm Configuration.

Semántica CURRENT:

```text
VALID
!=
FULLY RESOLVED
!=
READY
```

`VALID` significa intrínsecamente válido como Alarm Configuration.

La ausencia o drift de una Tool/evaluator puede impedir resolución/readiness posterior, pero no convierte automáticamente la revisión Source en inválida.

## 5. Family

### 5.1 Contrato CURRENT

**VERIFIED**

```python
AlarmIdentity(
    family_key: str,
    alarm_key: str,
)
```

`family_key`:

- es namespace lógico de Rules;
- participa en referencias;
- participa en resolución del evaluator;
- define scope de Messages FAMILY.

No existe un `FamilyDefinition` durable separado.

### 5.2 Runtime

**VERIFIED**

El lifecycle compartido del Engine no se agrupa por `family_key`.

Se agrupa por:

```text
priority_group
```

Por tanto:

```text
Family != Episode/Lifecycle Group
```

Una Family puede contener más de un `priority_group`.

### 5.3 Authoring

**PROPOSED / SAFE**

La UI puede usar Family como agregado visual derivado:

```text
Family
├── Priority Groups
├── Rules
└── Family Messages
```

sin introducir un nuevo documento durable.

No congelar `FamilyDefinition` mientras no exista necesidad independiente.

## 6. Identidad, nombre operativo y nombre humano

**VERIFIED / CURRENT**

Cada Rule distingue:

```text
family_key
alarm_key
rule_name
display_name
title
cause_template
```

Semántica:

```text
family_key
-> namespace lógico.

alarm_key
-> identidad estable/inmutable de la Rule.

rule_name
-> nombre técnico/operativo editable.
-> único dentro de Family.

display_name
-> friendly name humano.

title
-> título estático operacional.

cause_template
-> causa materializable con evidence/evaluation.
```

Campos históricos retirados:

```text
rule_key
content_key
title_template
image_key
```

`image_key`/imágenes permanecen diferidos, no deben reaparecer como placeholder.

### Authoring recomendado

```text
alarm_key
-> presentar como identidad estable.

rule_name
-> Operational name.

display_name
-> Friendly name.

title
-> título mostrado.

cause_template
-> explicación/cause dinámica.
```

## 7. Execution, visibility, clasificación y estado

### 7.1 Execution

**VERIFIED / CURRENT**

```text
is_active: bool
```

Semántica frozen:

```text
is_active=true
-> Rule participa del flujo ejecutable.

is_active=false
-> Rule permanece definida.
-> sale del flujo ejecutable.
-> si existe una occurrence abierta, adoption debe cerrarla como configuration-disabled.
```

La capacidad recordada históricamente como:

```text
not_execute
```

corresponde conceptualmente a:

```text
is_active=false
```

No introducir un segundo campo durable con semántica equivalente.

Importante:

```text
Runtime Configuration
!=
Execution Session
```

Una Rule disabled puede seguir representada en una materialización Runtime orientada a adoption para que el sistema conozca la transición, pero no debe formar parte de la nueva `AlarmExecutionSession`.

### 7.2 Operational visibility

**VERIFIED / CURRENT**

```text
visibility_mode:
VISIBLE
TRACE_ONLY
```

Semántica:

```text
VISIBLE
-> puede ser proyectada operacionalmente por Delivery.

TRACE_ONLY
-> continúa evaluándose.
-> continúa participando del Runtime conforme a su configuración.
-> continúa dejando trazabilidad.
-> Delivery no debe proyectarla visiblemente hacia las Tools.
```

La capacidad recordada históricamente como:

```text
not_visible
```

corresponde conceptualmente a:

```text
visibility_mode=TRACE_ONLY
```

No introducir un segundo boolean durable.

### 7.3 `delivery_enabled` histórico no equivale a TRACE_ONLY

**VERIFIED / CONFLICT TO RECONCILE**

`PlannedAlarm` CURRENT contiene:

```text
delivery_enabled: bool
```

pero dentro del Engine:

```text
delivery_enabled=false
-> PriorityDisposition.SHADOW
-> Rule deja de ser candidata operacional a predominancia
-> Management operacional no actúa como sobre una Rule delivery-enabled
```

Por tanto:

```text
TRACE_ONLY
!=
delivery_enabled=false
```

y B.2 no debe mapearlos automáticamente.

La semántica recuperada exige mantener separadas:

```text
EXECUTION
is_active

VISIBILITY
visibility_mode

PRIORITY
priority_group + priority_order

SPECIAL SEMANTICS
is_special_condition

ROUTING
criticality + escalation
```

Ninguna se infiere automáticamente de otra.

### 7.4 Clasificación

**VERIFIED / CURRENT**

```text
is_special_condition
kind
criticality
business_category
operational_areas
color
```

Valores:

```text
kind:
RISK
IMPACT

criticality:
C1
C2
C3

business_category:
ECOLOGY
PRODUCTIVITY
SAFETY_HEALTH
COSTS

operational_areas:
MINE
PLANT
(MINE, PLANT)

color:
RED
YELLOW
```

`kind` conserva significado de negocio.

`is_special_condition` es una dimensión independiente.

`criticality` gobierna routing C1/C2/C3, no prioridad relativa dentro del `priority_group`.

## 8. Evaluator y parameters

### 8.1 Contrato de authoring

**VERIFIED / CURRENT**

```text
evaluator_key
parameters
```

```python
Mapping[str, str | float | bool]
```

No soporta:

```text
int como tipo de dominio
None
list
tuple como valor
dict anidado
expresiones
código
```

Un número integral de negocio se representa como:

```text
1200.0
```

### 8.2 Runtime

**VERIFIED / CURRENT**

`AlarmEvaluatorRegistry` resuelve por:

```text
(family_key, evaluator_key)
```

`AlarmExecutionEntry` mantiene:

```text
planned_alarm
evaluator_contract
parameters
```

Los parámetros CURRENT del Runtime aceptan exactamente:

```text
str | float | bool
```

### 8.3 Metadata de parámetros

**VERIFIED ABSENCE / CURRENT**

No existe un schema general de parámetros por evaluator.

Los nombres y su semántica pertenecen al evaluator/desarrollador.

Por tanto, para UI V1 es correcto usar un editor genérico:

```text
Key
Type
Value
```

sin inventar schemas.

### 8.4 UI

**PROPOSED / SAFE**

Superar `Parameters JSON` con filas dinámicas:

```text
threshold_tph       Number      1200.0
window              Text        15m
quality_required    Boolean     Yes
```

Persistiendo exactamente el mapping durable.

## 9. Priority Group, ranking y Management suppression

### 9.1 Contrato

**VERIFIED / CURRENT**

```text
priority_group
priority_order
```

Cada `priority_group` tiene una única secuencia total.

Invariantes CURRENT:

```text
priority_order > 0
priority_order único dentro del group

si existen IMPACT + RISK:
todos los IMPACT preceden a todos los RISK
```

La última invariante sigue implementada en el Engine CURRENT y no se elimina desde este documento.

### 9.2 Priority resolution del Engine

**VERIFIED / CURRENT**

El Engine determina el candidato predominante por:

```text
min(priority_order)
```

entre Rules operacionalmente candidatas.

Esto ya soporta correctamente múltiples Rules del mismo `kind`.

Ejemplo:

```text
1 IMPACT A
2 IMPACT B
3 IMPACT C
4 RISK D
```

si todas están activas:

```text
A -> PREDOMINANT
B/C/D -> ECLIPSED o disposición operacional correspondiente
```

El ranking, no `kind`, decide entre Rules de la misma clase.

### 9.3 Evolución del proceso

**PROPOSED REFINEMENT / AGREED FOR CHARACTERIZATION**

El ranking representa una progresión operacional dentro del mismo `priority_group`.

Número menor:

```text
mayor prioridad / condición más crítica
```

Por tanto, si existe una gestión sobre rank 2 y posteriormente aparece rank 1:

```text
rank 1 DEBE poder emerger operacionalmente.
```

Una gestión de menor prioridad no puede silenciar una evolución hacia una condición más crítica.

### 9.4 Management suppression como barrera de ranking

**PROPOSED REFINEMENT / AGREED FOR CHARACTERIZATION**

Semántica recuperada:

```text
managed source priority = P

target.priority_order < P
-> NO suppressed por esa fuente

target.priority_order == P
-> source itself

target.priority_order > P
-> suppressed mientras el ManagementEffect conserve alcance
```

Ejemplo:

```text
1 active
2 managed
3 active
```

resultado esperado:

```text
1 -> visible/predominant
2 -> managed/hidden
3 -> suppressed by 2
```

Si rank 1 normaliza mientras el `ManagementEffect` de rank 2 continúa:

```text
2 sigue managed
3 sigue suppressed
```

hasta que:

- termina el ManagementEffect;
- rank 2 reaparece;
- aparece otra condición con prioridad superior;
- o el lifecycle normaliza/cierra según sus reglas.

Management suppression:

```text
NO cierra occurrence
NO detiene routing
NO convierte la condición física en inactiva
```

### 9.5 Gap CURRENT localizado

**VERIFIED / CONFLICT**

El mecanismo `CascadeSuppression` ya existe y conserva:

```text
source_alarm_identity
source_occurrence_id
management_effect_id
target_alarm_identity
```

`AlarmPriorityDecision` ya puede expresar:

```text
CASCADE_SUPPRESSED
blocking_alarm_identities
```

El acoplamiento histórico está concentrado en Management:

```text
source.kind == IMPACT
target.kind == RISK
target.priority_order > source.priority_order
```

Por tanto, el Engine central de priority ya usa ranking correctamente; el delta potencial está en cómo Management construye el conjunto de supresiones.

No modificar todavía producción.

Primero caracterizar con tests la matriz deseada.

### 9.6 UI

**PROPOSED / SAFE**

Representar `priority_group` como secuencia ordenada y no como números aislados.

El usuario administra ranking.

La UI persiste `priority_order`.

## 10. Special Conditions

### 10.1 Naturaleza

**VERIFIED / FROZEN**

Una Special Condition es una Rule normal marcada explícitamente:

```text
is_special_condition=true
```

No se deriva desde:

```text
priority_order
kind
criticality
```

y no es un tercer `AlarmKind`.

Una Rule rank 1 no se convierte automáticamente en Special Condition.

Una Rule rank 2 puede ser Special Condition si la configuración así lo declara.

### 10.2 Prioridad

**VERIFIED**

Special Conditions usan la misma secuencia:

```text
priority_group
priority_order
```

que todas las demás Rules.

No existe una liga separada.

### 10.3 Reappearance trigger

**VERIFIED / FROZEN**

Una Rule gestionada puede declarar:

```text
reappearance.special_conditions[]
```

como referencias explícitas por `AlarmIdentity`.

Regla:

```text
managed Rule X
AND main condition X remains active
AND referenced Special Condition becomes active
-> X REAPPEARS immediately
```

No importa si aún faltaba tiempo para `after_minutes`.

Con múltiples Special Conditions:

```text
SC1 OR SC2 OR SC3
```

cualquiera de las referenciadas puede disparar reappearance.

Una Special Condition no referenciada no produce ese efecto lateral.

### 10.4 Evaluation vs Delivery

**PROPOSED / REQUIRED BY SEMANTICS**

El trigger de Special Condition debe depender de su estado de evaluación/runtime, no de que sea visible en Delivery.

Una Special Condition podría ser:

```text
TRACE_ONLY
ECLIPSED
CASCADE_SUPPRESSED
```

y aun así su condición evaluada puede ser ACTIVE.

Por tanto:

```text
Special Condition trigger
!=
visible in a Tool
```

### 10.5 Special Cascade frozen vs recovered ranking semantics

B.1 frozen declaró:

```text
managed predominant Special Condition
-> suppress all other active Rules in the priority_group
```

El Engine CURRENT, en cambio, implementa la cascada histórica:

```text
managed IMPACT
-> suppress lower-priority RISK
```

La semántica recuperada en este milestone propone una simplificación más general:

```text
Management suppression
-> gobernada por priority_order para cualquier Rule gestionada

is_special_condition
-> agrega trigger semantics explícitas
-> no crea un segundo sistema de prioridad
```

Estado:

```text
PROPOSED REFINEMENT
NEEDS CHARACTERIZATION
NOT YET FROZEN
```

No alterar B.1 silenciosamente.

Si la characterization confirma esta semántica, deberá registrarse explícitamente como refinamiento/supersession parcial de la Special Cascade frozen.

### 10.6 Runtime gap

**VERIFIED / OPEN**

`PlannedAlarm` CURRENT no transporta:

```text
is_special_condition
reappearance.special_conditions
```

y el Engine CURRENT sólo conoce reappearance temporal mediante `ManagementEffect.reappearance_due_at`.

La conexión dinámica:

```text
Special Condition becomes active
-> release/reconcile ManagementEffect of referenced Rule
```

sigue siendo un gap real a diseñar después de characterization.

## 11. Management y reappearance

### 11.1 Authoring contract

**VERIFIED / CURRENT**

```python
ReappearanceDefinition(
    after_minutes: int | None,
    special_conditions: tuple[AlarmIdentity, ...],
)
```

```text
after_minutes=None
special_conditions=()
-> sin reappearance configurado
```

Semántica frozen:

```text
main condition remains active
AND
(
    timer elapsed
    OR
    any referenced Special Condition active
)
-> REAPPEAR
```

### 11.2 Timer reappearance del Engine

**VERIFIED / CURRENT**

El Engine tiene:

```text
ManagementEffect.reappearance_due_at
```

y reappearance temporal funcional.

Al vencer el efecto:

- mantiene la misma occurrence;
- incrementa `management_cycle`;
- limpia `management_effect`;
- recalcula prioridad con el estado activo vigente.

### 11.3 Fuente del due_at

**VERIFIED / GAP**

Runtime recibe:

```text
reappearance_due_at_resolver
```

como callable de composición.

Todavía no existe B.2 que derive ese resolver/materialización desde:

```text
AlarmDefinition.reappearance.after_minutes
```

### 11.4 Reappearance por Special Condition

**VERIFIED CONTRACT / RUNTIME GAP**

El contrato está frozen.

La materialización/runtime trigger todavía no existe.

No debe implementarse como una simulación de timer porque la Special Condition puede activarse dinámicamente después de iniciada la gestión.

### 11.5 Cambio de configuración durante gestión

B.1 frozen desea que cambios en:

```text
after_minutes
special_conditions
```

se reconcilien contra el `ManagementEffect` vigente.

Esto todavía no está materializado en `PlannedAlarm`/Runtime.

### 11.6 Routing durante Management

**VERIFIED**

```text
Management oculta/suprime
!=
routing detenido
```

Los clocks C1/C2/C3 continúan conforme a su semántica.

## 12. Deactivation de la Rule

### 12.1 Authoring contract

**VERIFIED / CURRENT**

```python
AlarmDeactivationDefinition(
    enabled: bool,
    max_duration_hours: int | None,
    approval_required: bool,
)
```

Invariantes:

```text
enabled=false
-> max_duration_hours=None
-> approval_required=false

enabled=true
-> max_duration_hours 1..12
```

### 12.2 Engine CURRENT

**VERIFIED**

`PlannedAlarm` sólo recibe:

```python
DeactivationPolicy(
    approval_required: bool
)
```

La intención operacional llega como:

```python
DeactivationIntent(
    effective_until
)
```

Por tanto el Engine CURRENT:

- conoce si requiere aprobación;
- materializa request/decision/effect;
- no conoce directamente `max_duration_hours` de AlarmDefinition;
- no calcula por sí mismo el máximo permitido desde esa configuración.

### 12.3 Approval

**VERIFIED / CURRENT ENGINE**

El Engine sí soporta:

```text
approval_required=false
-> DIRECT
-> DeactivationEffect inmediato

approval_required=true
-> PENDING_APPROVAL
-> durable DeactivationRequest
-> DeactivationDecision
-> APPLIED / REJECTED / CANCELLED / etc.
```

Esto está cubierto por tests CURRENT.

Por tanto, la frase correcta es:

```text
Approval domain flow:
IMPLEMENTED en Engine/Core

Approval application/user workflow:
NO debe asumirse compuesto sólo por existir el dominio
```

### 12.4 Max duration / shift end

**OPEN / B.2-DELIVERY BOUNDARY**

B.1 define el máximo configurable.

La decisión frozen plantea conceptualmente:

```text
effective_until =
min(
    operator_selected_until,
    now + configured_max_duration,
    shift_end
)
```

El Engine recibe ya un `effective_until`.

Esto sugiere que el cálculo/capability efectivo debe resolverse antes del Engine, no necesariamente dentro del lifecycle.

No modificar Engine para esto sin necesidad demostrada.

## 13. Message Catalog

### 13.1 Contrato

**VERIFIED / CURRENT**

```python
MessageDefinition(
    message_key,
    scope,
    family_key,
    display_text,
    is_active,
    deactivation_override,
)
```

Scopes:

```text
GLOBAL
FAMILY
```

Reglas:

```text
GLOBAL -> family_key=None
FAMILY -> family_key requerido
```

`message_key` es identidad.

### 13.2 Asociación

**VERIFIED**

Cada Rule selecciona explícitamente:

```text
message_keys[]
```

Una Rule puede elegir:

```text
GLOBAL
+
misma FAMILY
```

Nunca otra Family.

Crear un Message nuevo no altera Rules existentes.

### 13.3 Message deactivation override

**VERIFIED CONTRACT**

Si:

```text
deactivation_override=None
```

se usa default de la Rule.

Si existe override:

```text
reemplaza COMPLETAMENTE el default
```

Puede incluso permitir un máximo mayor al default de la Rule, limitado por las restricciones operacionales posteriores.

### 13.4 Engine CURRENT

**VERIFIED / GAP**

`MessageDefinition` no forma parte de `PlannedAlarm` CURRENT.

`PlannedAlarm.deactivation_policy` contiene únicamente:

```text
approval_required
```

Por tanto la resolución:

```text
Rule default
+
selected Message
+
Message override
->
effective deactivation capability
```

sigue perteneciendo a B.2/Delivery.

### 13.5 Multiple Messages con overrides

**OPEN**

No existe una regla congelada recuperada para decidir automáticamente cómo combinar varios Messages seleccionados si tienen overrides distintos.

No inventar:

```text
min
max
first
last
merge
```

La UI puede permitir configurar los Messages, pero la semántica efectiva necesita una decisión antes de materialización.

## 14. Routing y criticality

### 14.1 Authoring source contract

**VERIFIED / CURRENT**

```text
origin_tool_key

steps[]
    step_order
    target_tool_key
    is_enabled
    wait_minutes_from_previous_step
```

### 14.2 PlannedAlarm / Engine contract

**VERIFIED / CURRENT**

```python
AlarmRouting(
    origin_tool_key,
    destinations: tuple[RoutingDestination, ...]
)

RoutingDestination(
    tool_key,
    delay_seconds,
)
```

### 14.3 Semántica C1/C2/C3

**VERIFIED / TESTED**

```text
C1
-> origin + todos los destinos configurados/asociados
-> asignación inmediata
-> destination.delay_seconds=None

C2
-> origin inmediato
-> cada destination aparece según su deadline efectivo
-> destination.delay_seconds requerido

C3
-> vive sólo en origin
-> no tiene destinations
-> nunca escala
```

C1 no significa “todas las Tools existentes”; significa todos los destinos válidos configurados para esa Rule.

### 14.4 Configuración estática vs flujo caliente

Alarm Configuration declara intención:

```text
origin
destinations
waits
criticality
```

No declara estado caliente como:

```text
ahora está en IO
ya escaló
timer venció
destination assigned
```

Eso pertenece al Engine/Runtime y a la Live Projection.

### 14.5 Routing durante eclipse/management/suppression

**VERIFIED**

El routing continúa progresando aunque la Rule:

```text
esté managed
esté eclipsed
esté cascade-suppressed
```

conforme al contrato vigente.

### 14.6 Mutation/adoption CURRENT

**VERIFIED**

```text
criticality change
-> STRUCTURAL_RESET

C2 routing changes
-> COMPATIBLE

C1 routing mutation
-> REJECTED

C3 routing mutation
-> REJECTED
```

## 15. Semántica de waits C2: configured vs effective

### 15.1 Configuración humana

B.1 define:

```text
wait_minutes_from_previous_step
```

La intención de authoring recuperada es secuencial:

```text
Origin
  ↓ 10 min
Integrated Operations
  ↓ 20 min
Strategic
```

configurado como:

```text
IO        wait_from_previous = 10
Strategic wait_from_previous = 20
```

### 15.2 Contrato Engine

**VERIFIED / CURRENT**

Engine utiliza:

```text
RoutingDestination.delay_seconds
```

como deadline absoluto desde el inicio de la occurrence.

### 15.3 Materialización propuesta

**PROPOSED / STRONGLY ALIGNED WITH EXISTING ENGINE**

B.2 debería convertir waits relativos configurados en delays efectivos acumulados:

```text
Configured                       Effective Runtime

IO        +10 min                occurrence +10 min
Strategic +20 min desde IO       occurrence +30 min
```

Formalmente:

```text
effective_delay(step N)
=
sum(enabled waits desde origin hasta N)
```

Ejemplo:

```text
10 + 20 = 30 min
```

El durable authoring conserva únicamente los valores humanos relativos.

Los valores efectivos son derivados/materializados y pueden exponerse read-only para diagnóstico o preview.

No agregar un segundo campo durable de “effective delay” a `AlarmDefinition`.

### 15.4 Estado

```text
Engine absolute deadlines       CURRENT / VERIFIED
Authoring relative waits        CURRENT contract wording
Relative -> cumulative mapping  PROPOSED / TO FREEZE IN B.2
```

Esta solución evita modificar routing Engine y evita ensuciar la configuración.

## 16. Routing por Tool tier

### 16.1 Tool kinds

**VERIFIED / CURRENT**

```text
PROCESS
INTEGRATED_OPERATIONS
STRATEGIC
```

### 16.2 Memoria operacional recuperada

Se recuerda:

```text
PROCESS
    ↓
INTEGRATED_OPERATIONS
    ↓
STRATEGIC

nunca hacia atrás
```

### 16.3 Resultado del rastrillo

**UNVERIFIED**

La matriz anterior no está congelada en:

- B.1 frozen;
- B.2 recorded;
- Engine CURRENT.

Engine trata routing como `tool_key` opaco y no conoce `ToolConfigurationKind`.

B.1 dice explícitamente que:

```text
routing/escalation válido para criticality y Tool types
```

es responsabilidad de B.2.

Por tanto:

```text
PROCESS -> IO -> STRATEGIC
NO puede marcarse todavía CURRENT
```

hasta recuperar una fuente adicional o adoptar una nueva decisión explícita.

### 16.4 Implicación

La jerarquía de tiers, si se confirma, debe implementarse en:

```text
B.2 / Tool compatibility resolution
```

no dentro del lifecycle Engine.

## 17. Mine / Plant y Tool scopes

### 17.1 Rule

**VERIFIED**

```text
operational_areas:
MINE
PLANT
MINE + PLANT
```

### 17.2 Process Tool

**VERIFIED / CURRENT**

`ToolStructure` exige:

```text
PROCESS
-> operational_scope obligatorio
   MINE o PLANT
```

### 17.3 Integrated Operations

**VERIFIED / CURRENT**

Integrated Operations:

```text
no tiene un único operational_scope
```

Sus Components declaran:

```text
scope = MINE | PLANT
```

### 17.4 Strategic

**VERIFIED**

`STRATEGIC` existe como Tool kind.

No existe visual alarm projection definida para Strategic.

### 17.5 Compatibilidad Rule -> Tool

**INFERRED / NEEDS B.2 DECISION**

Es coherente que una Rule `MINE` sólo ofrezca Process `MINE`, y equivalente para `PLANT`.

Pero la matriz exacta no está implementada en B.2.

Para `(MINE, PLANT)` también falta congelar si:

- se permiten ambos Process;
- se exige Integrated Operations;
- depende de origin/target.

No inventar esta regla en callbacks.

## 18. Tool Catalog y Alarm Tool References

### 18.1 Tool Catalog

**VERIFIED / CURRENT**

Cada entry conserva:

```text
tool_key
display_name
kind
source_release_id
ToolStructure
```

Por tanto la información de scopes sí existe upstream.

### 18.2 AlarmToolReferenceReader

**VERIFIED / CURRENT GAP**

El read model de authoring entrega:

```text
tool_key
display_name
kind
source_release_id
components
subcomponents
```

No conserva:

```text
PROCESS operational_scope
INTEGRATED_OPERATIONS component.scope
```

Además omite completamente:

```text
STRATEGIC
```

porque Strategic no tiene visual projection de alarmas.

### 18.3 Refinamiento necesario

**PROPOSED**

Separar necesidades:

```text
Routing reference view
-> puede requerir STRATEGIC
-> necesita kind/scope

Visual projection reference view
-> sólo Tools con visual projection definida
-> Component/Subcomponent topology
```

No modificar Tool Catalog durable para esto: ya contiene `ToolStructure`.

El cambio correcto está en el read model/authoring boundary.

## 19. Visual Targets

### 19.1 Contract

**VERIFIED / CURRENT**

```python
AlarmVisualTarget(
    tool_key,
    component_keys,
    subcomponents,
    process_projection_mode,
)
```

Subcomponent identity:

```text
(owner_component_key, subcomponent_key)
```

### 19.2 Integrated Operations

**VERIFIED**

```text
Component
-> dónde se posiciona/organiza la alarma

Subcomponents
-> qué recibe color/afectación
```

Una Rule puede seleccionar múltiples Components y múltiples Subcomponents.

### 19.3 Process

**VERIFIED**

Cada target PROCESS declara:

```text
GENERIC
DISTRIBUTED
```

La geometría concreta del panel no pertenece al Engine.

### 19.4 Strategic

**VERIFIED UNDEFINED**

No existe todavía comportamiento visual específico de Strategic.

No inventarlo.


### 19.5 Presentation family vs Process projection variant

**PARTIALLY VERIFIED / TERMINOLOGY UNVERIFIED**

`GENERIC/DISTRIBUTED` no es routing ni Tool kind.

Es una variante de presentación declarada por cada target `PROCESS`.

B.1 conserva evidencia concreta de una Tool Process donde:

```text
0 distributed activas
-> slots normales

1 distributed activa
-> participa del flujo normal

2+ distributed activas
-> aparece un agrupador distributed
```

La terminología recuperada para la familia visual superior es:

```text
PROCESS
-> carousel
   -> GENERIC / DISTRIBUTED como variante

INTEGRATED_OPERATIONS
-> queue-in-queue

STRATEGIC
-> undefined
```

Los nombres `carousel` y `queue-in-queue` no están verificados en contratos CURRENT y por ahora son vocabulario de producto recuperado, no enums durables.

Mantener separadas:

```text
Routing
-> cuándo/a qué Tool llega la occurrence

Visual Target
-> dónde impacta dentro de la Tool

Presentation family
-> cómo esa Tool organiza visualmente alarmas

Process projection variant
-> GENERIC / DISTRIBUTED para esa Rule en ese target PROCESS
```

## 20. Adoption — desired vs CURRENT

B.1 congeló una matriz deseada de evolución de configuración.

El Engine CURRENT aún conserva varias restricciones históricas.

| Cambio | B.1 desired | Engine CURRENT |
|---|---|---|
| `rule_name` | COMPATIBLE | fuera de PlannedAlarm |
| `display_name` | COMPATIBLE | Delivery, fuera de PlannedAlarm |
| `title/cause` | COMPATIBLE | Delivery |
| `parameters` | COMPATIBLE | COMPATIBLE |
| `priority_order` | COMPATIBLE | COMPATIBLE |
| `criticality` | STRUCTURAL_RESET | STRUCTURAL_RESET |
| C2 routing | COMPATIBLE | COMPATIBLE |
| `evaluator_key` | COMPATIBLE deseado | REJECTED |
| `kind` | COMPATIBLE deseado | REJECTED |
| `priority_group` | structural group migration | REJECTED |
| `origin_tool_key` | STRUCTURAL_RESET | no tiene clasificación específica; entra como routing mutation |
| C1 routing mutation | no congelado como compatible | REJECTED |
| C3 routing mutation | no congelado como compatible | REJECTED |
| visual targets | COMPATIBLE | fuera del Engine lifecycle |
| process projection mode | COMPATIBLE | fuera del Engine lifecycle |
| Special Condition semantics | reconcile | no materializada |

### Finding adicional: origin Tool

B.1 frozen dice:

```text
origin_tool_key
-> STRUCTURAL_RESET
```

Engine adoption CURRENT no inspecciona `origin_tool_key` por separado.

Para C2, un cambio de `routing` que no cae en un rejection específico termina clasificado como compatible.

Por tanto existe un posible conflicto:

```text
B.1 desired origin change = STRUCTURAL_RESET
vs
Engine CURRENT C2 routing mutation = COMPATIBLE
```

Debe verificarse/decidirse en B.2/adoption antes de permitir promoción operacional de ese cambio.

## 21. B.2 historical vs CURRENT canonical

### 21.1 Invariantes históricas que siguen siendo útiles

B.2 recorded aporta:

```text
INVALID != REMOVED

DISABLED != INVALID

Runtime y Delivery derivan de una misma resolución

Delivery no puede adelantarse al Runtime

Runtime adoption determina EFFECTIVE

Live Projection != Management Projection

Web operacional no resuelve prioridad ni Message catalogs
```

Estas invariantes siguen alineadas con canonical.

### 21.2 Strict persistence gate de Increment 2

Increment 2 histórico endureció:

```text
LATEST SAVED = LATEST VALID
```

pero entendiendo VALID de forma que referencias externas podían bloquear persistence.

Canonical CURRENT refinó esa semántica:

```text
LATEST SAVED = LATEST INTRINSICALLY VALID

VALID
!= FULLY RESOLVED
!= READY
```

Por tanto:

```text
B.2 Increment 2 strict external pre-save gate
-> SUPERSEDED / REFINED
```

No reintroducirlo desde la UI.

### 21.3 Preconfiguration CURRENT

**VERIFIED / CURRENT**

Se permite persistir una Rule con:

```text
Tool aún no disponible
o
evaluator aún no disponible
```

si el aggregate es intrínsecamente válido.

La referencia:

```text
no se borra
no se vuelve disabled
no se convierte en removed
```

Podrá re-resolverse posteriormente.

## 22. Runtime / Delivery boundary

**VERIFIED DIRECTION**

B.2 debe producir, desde una misma resolución/provenance:

```text
Runtime materialization
Delivery materialization
```

### 22.1 Runtime

Runtime necesita, según capability:

- executable Rule;
- evaluator reference;
- parameters;
- priority;
- routing;
- management/reappearance inputs;
- adoption semantics.

### 22.2 Delivery

Delivery necesita:

- display/title/cause;
- color;
- kind/criticality/category/areas;
- Messages;
- deactivation capability;
- visual targets;
- Process projection mode;
- visibility policy.

### 22.3 Disabled vs trace-only

Debe distinguirse:

```text
is_active=false
-> Rule definida pero no ejecutable en la nueva execution session.

is_active=true + TRACE_ONLY
-> Rule ejecutable y trazable.
-> no visible en Delivery.
```

No confundir esto con:

```text
PlannedAlarm.delivery_enabled=false
```

porque CURRENT Engine lo trata como `SHADOW` y altera prioridad/management.

### 22.4 Readiness

Runtime y Delivery pueden tener readiness distinta.

Una dependencia exclusivamente visual puede dejar:

```text
Runtime READY
Delivery NOT READY
```

sin impedir evaluación.

Delivery nunca lidera Runtime ni modifica semántica lifecycle para resolver una preocupación visual.

## 23. Qualification y política de intervención del Engine

**VERIFIED / CURRENT DIRECTION**

R3.5 qualification cerró F-010:

```text
CLOSED PASS/GREEN
1000 alarms
361/361 iterations
0 overruns
journal aligned
management requests 480/480
management decisions 480/480
no open product findings
```

La campaña también cubrió:

- routing;
- management;
- deactivation;
- adoption;
- WAL/recovery;
- leases/fencing;
- drain;
- soak/stress.

Regla de trabajo:

> No modificar Alarm Engine sólo para hacer más simple el authoring.

Antes de tocar Engine clasificar una necesidad como:

```text
A. AlarmDefinition
B. B.2 resolution
C. Tool Configuration
D. evaluator
E. Delivery/UI
F. gap real del Engine
```

Sólo `F` justifica discutir cambio del Engine.

## 24. Modelo de authoring recomendado

**PROPOSED / NOT YET FROZEN UI**

```text
Alarm Configuration
|
+-- Global Messages
|
+-- Families
    |
    +-- Family
        |
        +-- Overview
        |
        +-- Family Messages
        |
        +-- Priority Groups
        |   |
        |   +-- ordered Rules
        |
        +-- Rules
            |
            +-- Identity & presentation
            +-- Classification
            +-- Evaluation
            +-- Priority
            +-- Messages
            +-- Management / Reappearance
            +-- Routing
            +-- Visual Projection
```

Family es una vista derivada.

No crea un nuevo contrato durable.

## 25. Implicaciones seguras para UI

Puede hacerse sin esperar B.2:

```text
Family-first navigation
Master/detail para Rules
Master/detail para Messages
Priority Groups como secuencia
Parameters tipados dinámicos
Selector GLOBAL + FAMILY Messages
Deactivation fields con progressive disclosure
Reappearance timer input
Special Condition selector limitado por same family/group en aggregate
Visual targets como Tool -> Component -> Subcomponent
Friendly display names conservando keys durables
```

No debe implementarse todavía como regla definitiva:

```text
PROCESS -> IO -> STRATEGIC
nunca hacia atrás
```

hasta congelar tier routing.

Tampoco debe calcularse silenciosamente:

```text
effective Message deactivation
```

cuando múltiples Messages tienen overrides distintos.

## 26. OPEN después de la consolidación

### OPEN-1 — Management suppression characterization

Cerrar la semántica recuperada:

```text
Management suppression governed by priority_order
not by IMPACT -> RISK eligibility
```

Caracterizar, antes de producción:

- rank 1 managed -> rank 2 suppressed;
- rank 2 managed -> rank 1 puede emerger;
- IMPACT -> IMPACT;
- RISK -> RISK si aplica la regla universal;
- source occurrence cerrada con ManagementEffect aún vigente;
- múltiples suppression sources;
- suppression release;
- interacción con routing y deactivation.

### OPEN-2 — Special Condition runtime trigger

Diseñar la conexión:

```text
referenced Special Condition ACTIVE
-> managed Rule reappears immediately
```

sin depender de Delivery visibility.

### OPEN-3 — Routing tier matrix

Confirmar:

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC
```

y:

- backward prohibited;
- direct PROCESS -> STRATEGIC permitido o no;
- IO origin permitido;
- Strategic origin permitido.

### OPEN-4 — C2 wait materialization

Congelar:

```text
wait_minutes_from_previous_step
-> cumulative absolute delay_seconds
```

como contrato B.2.

### OPEN-5 — Rule area vs Tool scope

Congelar compatibilidad:

```text
Rule MINE / PLANT / BOTH
vs
PROCESS operational_scope
vs
Integrated component.scope
```

separando routing de visual projection.

### OPEN-6 — Multiple Message overrides

Definir semántica efectiva cuando una Rule selecciona múltiples Messages con overrides distintos.

### OPEN-7 — Origin Tool adoption

Resolver:

```text
B.1: origin change = STRUCTURAL_RESET
Engine CURRENT C2 routing mutation = COMPATIBLE
```

### OPEN-8 — Effective deactivation capability

Definir:

```text
Rule default
+
Message override
+
operator selected until
+
configured max
+
shift end
->
effective capability / effective_until
```

### OPEN-9 — Routing references vs visual references

Refinar `AlarmToolReferenceReader` para:

- conservar scopes;
- permitir Strategic donde routing lo requiera;
- no usar exclusión visual como exclusión de routing.

### OPEN-10 — Runtime/Delivery mapping de visibility

Congelar que:

```text
TRACE_ONLY
```

permanezca ejecutable/trazable sin mapearse ciegamente a:

```text
delivery_enabled=false
```

### OPEN-11 — Presentation terminology

Confirmar nombres productivos:

```text
carousel
queue-in-queue
```

sin crear enums durables antes de tener evidencia/uso contractual.

### OPEN-12 — B.2 concrete contract

Definir:

```text
ResolvedAlarmConfiguration
resolution identity/provenance
Runtime readiness
Delivery readiness
findings
materialization boundaries
```

sin reintroducir el external pre-save gate superseded.

## 27. Matriz de estado consolidada

| Tema | Estado |
|---|---|
| AlarmConfiguration aggregate | CURRENT / VERIFIED |
| AlarmDefinition B.1 shape | CURRENT / IMPLEMENTED |
| `is_active=false` = no execution | CURRENT / VERIFIED |
| `TRACE_ONLY` = execute + trace, no visible Delivery | CURRENT / VERIFIED |
| `TRACE_ONLY == delivery_enabled=false` | FALSE / CONFLICT TO RECONCILE |
| Family por `family_key` | CURRENT |
| FamilyDefinition durable | NOT PRESENT / NOT NEEDED YET |
| Priority Group | CURRENT / ENGINE ALIGNED |
| priority predominance by `priority_order` | CURRENT / TESTED |
| IMPACT-before-RISK invariant | CURRENT / ENGINE ALIGNED |
| Management suppression by rank | PROPOSED / CHARACTERIZATION NEXT |
| Historical IMPACT -> lower RISK cascade | CURRENT ENGINE |
| Special Condition explicit flag | CURRENT / FROZEN |
| Special Condition inferred from ranking | FORBIDDEN |
| Special Condition reappearance contract | FROZEN |
| Special Condition runtime trigger | OPEN |
| Generic parameter metadata | NOT PRESENT |
| C1 immediate routing | CURRENT / TESTED |
| C2 delayed routing | CURRENT / TESTED |
| C3 origin only | CURRENT / TESTED |
| C2 relative waits -> cumulative delays | PROPOSED B.2 |
| C2 routing mutation | CURRENT / COMPATIBLE |
| C1/C3 routing mutation | CURRENT / REJECTED |
| Criticality mutation | CURRENT / STRUCTURAL_RESET |
| Kind mutation | CURRENT / REJECTED; B.1 desired differs |
| Evaluator mutation | CURRENT / REJECTED; B.1 desired differs |
| Priority group mutation | CURRENT / REJECTED; B.1 desired differs |
| Origin Tool mutation | CONFLICT / OPEN |
| Timer reappearance engine | CURRENT |
| AlarmDefinition -> timer mapping | OPEN |
| Rule deactivation definition | CURRENT |
| Engine approval flow | CURRENT / TESTED |
| Rule max duration -> effective_until | OPEN B.2/Delivery |
| GLOBAL/FAMILY Messages | CURRENT |
| Message override contract | CURRENT |
| Multiple override resolution | OPEN |
| Tool Catalog | CURRENT |
| PROCESS scope | CURRENT |
| Integrated component scope | CURRENT |
| STRATEGIC kind | CURRENT |
| Strategic visual projection | UNDEFINED |
| GENERIC/DISTRIBUTED | CURRENT PROCESS TARGET CONTRACT |
| carousel / queue-in-queue naming | UNVERIFIED PRODUCT TERMINOLOGY |
| Routing tier matrix | UNVERIFIED |
| Alarm Tool Reference scope preservation | GAP |
| Routing/visual reference separation | PROPOSED |
| B.2 | PLANNED |
| Engine qualification baseline | CLOSED PASS/GREEN |
| Broad Engine rewrite | BLOCKED |

## 28. Invariantes congeladas para el siguiente trabajo

Hasta decisión explícita en contrario:

```text
AlarmDefinition es el contrato editable.

AlarmConfiguration persiste Rules + Messages atómicamente.

VALID != FULLY RESOLVED != READY.

AlarmIdentity = family_key + alarm_key.

alarm_key es identidad estable.

rule_name y display_name son distintos.

is_active controla participación ejecutable.

TRACE_ONLY controla visibilidad Delivery, no existencia Runtime.

TRACE_ONLY no debe mapearse ciegamente a Engine SHADOW.

Family no equivale a priority_group.

priority_group es el scope de lifecycle/priority.

Existe una sola secuencia priority_order por group.

El Engine CURRENT selecciona predominancia por menor priority_order.

IMPACT debe preceder RISK bajo el contrato CURRENT.

is_special_condition es explícito y no se deriva de ranking.

Una Special Condition referenciada puede disparar reappearance aunque no sea visible en Delivery.

Parameters son str | float | bool.

Messages se seleccionan explícitamente por Rule.

Message override reemplaza completamente default deactivation.

Management no cierra por sí mismo la condición física.

Management no detiene routing.

C1 es inmediato hacia todos sus destinos configurados.

C2 usa destinos retardados.

C3 es origin only y nunca escala.

Alarm Configuration describe intención estática; Runtime conoce estado caliente.

Routing y Visual Projection son contratos distintos.

Integrated Operations Component posiciona.

Subcomponents determinan afectación visual.

Subcomponent identity conserva owner_component_key.

Process target usa GENERIC/DISTRIBUTED.

Strategic visual behavior no se inventa.

Tool Configuration mantiene ownership de topology/scope.

Alarm Configuration guarda referencias, no duplica Tool Configuration.

Tool/evaluator unresolved no vuelve intrínsecamente inválida la Alarm Source revision.

Runtime adoption determina EFFECTIVE.

Delivery no lidera Runtime.

No modificar Engine por conveniencia de authoring.

Cualquier cambio de Management suppression debe preservarse mediante characterization tests antes de producción.
```

## 29. Conflictos que canonical debe mantener visibles

No resolver silenciosamente:

```text
1. Management suppression recuperada por priority_order
   vs
   Engine CURRENT cascade IMPACT -> lower RISK.

2. B.1 Special Cascade:
   managed predominant Special Condition -> all other Rules
   vs
   propuesta de suppression uniforme por ranking.

3. B.1 reappearance.special_conditions
   vs
   ausencia de trigger dinámico en Runtime CURRENT.

4. TRACE_ONLY = execute + trace, no visible Delivery
   vs
   PlannedAlarm.delivery_enabled=false = SHADOW en Engine CURRENT.

5. B.1 origin_tool_key STRUCTURAL_RESET
   vs
   Engine CURRENT C2 routing mutation compatible.

6. B.1 desired compatible kind/evaluator changes
   vs
   Engine CURRENT REJECTED.

7. B.1 desired priority_group migration
   vs
   Engine CURRENT REJECTED.

8. B.2 Increment 2 strict external pre-save gate
   vs
   canonical CURRENT intrinsic-valid persistence.
```

## 30. Próximo foco único

El modelo está suficientemente consolidado para dejar de ampliar authoring de forma horizontal.

Siguiente incremento:

```text
ALARM-MANAGEMENT-SUPPRESSION-CHARACTERIZATION
```

Objetivo:

```text
demostrar con tests el comportamiento CURRENT
y el delta exacto necesario para que priority_order
sea la autoridad de Management suppression.
```

Alcance:

```text
tests/characterization
priority
management
cascade suppression
reappearance interaction
routing continuity
```

No incluye:

```text
B.2 completo
UI final
Tool tier routing
Message override resolution
broad Engine refactor
```

Criterio de salida:

```text
1. matriz CURRENT documentada;
2. tests que prueban qué ya funciona;
3. tests que fallan únicamente por el coupling IMPACT/RISK;
4. delta productivo mínimo identificado;
5. confirmación de que lifecycle/routing/deactivation no requieren rediseño.
```

Después:

```text
si el delta es localizado
-> implementar incremento mínimo del Engine

si aparecen dependencias amplias
-> volver a diseño antes de modificar producción
```

Una vez estabilizada Management suppression, el siguiente frente podrá ser:

```text
Special Condition runtime reappearance
```

y luego:

```text
B.2 materialization + UI composition
```

para empezar a validar el flujo completo en vivo.
