# ADA Command Center — Alarm Configuration Authoring Model

Estado: **DRAFT / AUTHORITY RECONCILED / OPEN CONTRACTS REMAIN**

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

## 7. Clasificación y estado

**VERIFIED / CURRENT**

```text
is_active
visibility_mode
is_special_condition
kind
criticality
business_category
operational_areas
color
```

Valores:

```text
visibility_mode:
VISIBLE
TRACE_ONLY

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

`TRACE_ONLY`:

```text
se evalúa y traza
pero Delivery no debe mostrarla como alarma visible
```

`is_active=false`:

```text
Rule sigue definida
pero no forma parte de la execution session
```

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

## 9. Priority Group y prioridad

### 9.1 Contrato

**VERIFIED / CURRENT**

```text
priority_group
priority_order
```

Cada `priority_group` tiene una única secuencia total.

Invariantes presentes tanto en Alarm Configuration como en Engine:

```text
priority_order > 0
priority_order único dentro del group

si existen IMPACT + RISK:
todos los IMPACT preceden a todos los RISK
```

### 9.2 Engine

**VERIFIED / CURRENT**

El Engine vuelve a validar la unicidad/orden del execution plan.

Priority se resuelve antes de Live Projection.

La Web operacional no decide predominancia.

### 9.3 UI

**PROPOSED / SAFE**

El usuario debería administrar una lista ordenada, no escribir principalmente números:

```text
Priority Group: CRUSHING

1  Crusher Trip
2  Crusher Throughput Risk
3  Low Stockpile
```

La UI deriva/persiste `priority_order`.

## 10. Special Conditions — contrato vs Engine

Este es el conflicto más importante encontrado.

### 10.1 B.1 frozen / AlarmDefinition

**VERIFIED CONTRACT**

Una Special Condition es una Rule normal con:

```text
is_special_condition=true
```

No es un tercer `AlarmKind`.

Debe conservar:

- kind;
- criticality;
- evaluator;
- parameters;
- priority;
- routing;
- management;
- visual targets.

Una referencia de reappearance debe apuntar a una Rule:

```text
misma Family
AND
mismo priority_group
AND
is_special_condition=true
```

Alarm Configuration CURRENT ya valida esas relaciones intrínsecas.

### 10.2 Semántica frozen deseada

B.1 congeló la intención:

```text
managed predominant Special Condition
-> Special Cascade
-> bloquea las demás Rules activas del mismo priority_group
```

No debía limitarse sólo a RISK.

### 10.3 Engine CURRENT

**VERIFIED / CONFLICT**

`PlannedAlarm` CURRENT no contiene:

```text
is_special_condition
special_conditions
reappearance definition
```

El Engine actual implementa la cascada histórica mediante:

```text
source_plan.kind == IMPACT
AND source_plan.delivery_enabled
```

y sólo suprime targets:

```text
kind == RISK
AND lower priority than source IMPACT
AND delivery_enabled
```

Por tanto:

```text
B.1 Special Condition semantics
!=
Engine CURRENT cascade semantics
```

### 10.4 Estado

```text
AlarmDefinition.is_special_condition          CURRENT
AlarmConfiguration validation                 CURRENT
B.1 desired Special Cascade                   FROZEN
Runtime materialization of is_special_condition  NOT IMPLEMENTED
Engine current cascade                        HISTORICAL IMPACT -> lower RISK
```

Estado global:

```text
CONFLICT / B.2 + ENGINE RECONCILIATION OPEN
```

No modificar Engine todavía.

Primero debe decidirse si B.2:

- adapta el contrato a la semántica Engine existente;
- o si existe evidencia suficiente para evolucionar Engine hacia la semántica frozen.

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

### 11.2 Engine timer reappearance

**VERIFIED / CURRENT**

El Engine tiene:

```text
ManagementEffect.reappearance_due_at
```

y reappearance temporal funcional.

Al vencer el efecto:

- mantiene la misma occurrence;
- incrementa `management_cycle`;
- limpia `management_effect`.

La qualification incluye management intensivo y cierre F-010.

### 11.3 Fuente del due_at

**VERIFIED / GAP**

El Runtime recibe:

```text
reappearance_due_at_resolver
```

como callable de composición.

No existe todavía B.2 que derive ese resolver/materialización desde:

```text
AlarmDefinition.reappearance.after_minutes
```

Por tanto:

```text
timer behavior exists in Engine
pero AlarmDefinition -> timer mapping sigue OPEN
```

### 11.4 Reappearance por Special Condition

**VERIFIED ABSENCE / GAP**

No existe en `PlannedAlarm` CURRENT una lista de Special Conditions para reappearance.

No existe materialización B.2 que conecte:

```text
AlarmDefinition.reappearance.special_conditions
```

con Engine.

Estado:

```text
PLANNED / RECONCILIATION OPEN
```

### 11.5 Cambio de after_minutes durante una gestión

B.1 frozen desea:

```text
cambio de after_minutes
-> recalcular due del ManagementEffect actual
```

No existe evidencia en `main` de reconciliación de este campo porque no forma parte de `PlannedAlarm`.

Estado:

```text
FROZEN DESIRED / NOT CURRENTLY MATERIALIZED
```

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

Invariantes locales:

```text
origin no vacío
step_order > 0
step_order único
target no vacío
target != origin
targets sin duplicados
wait is None OR >= 0
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
    delay_seconds
)
```

Semántica exacta CURRENT:

```text
C1
-> todos los destinations deben ser inmediatos
-> delay_seconds=None

C2
-> todos los destinations requieren delay_seconds

C3
-> no puede tener destinations
-> origin only
```

### 14.3 Runtime behavior

**VERIFIED / TESTED**

C1:

```text
origin + destinations
-> asignados inmediatamente
```

C2:

```text
origin
-> inmediato

destinations
-> scheduled
```

Los deadlines C2 se calculan como tiempos absolutos desde el inicio de la occurrence.

Ejemplo probado:

```text
destination B: 900s
-> occurrence start + 15m

destination C: 1800s
-> occurrence start + 30m
```

C3:

```text
origin only
```

### 14.4 Routing durante eclipse/management

**VERIFIED**

El routing sigue progresando aunque una RISK esté eclipsada por un IMPACT activo.

La decisión B.1 también retiró:

```text
continue_escalation_clock_when_hidden
```

porque el reloj continúa.

### 14.5 Mutation/adoption CURRENT

**VERIFIED**

Engine adoption:

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

## 15. Semántica de tiempos de escalamiento

Este es un OPEN importante.

### Authoring

B.1 usa:

```text
wait_minutes_from_previous_step
```

lo que lingüísticamente describe espera desde el paso anterior.

### Runtime

Engine usa:

```text
delay_seconds
```

como deadline absoluto desde el inicio de la occurrence.

### Ejemplo del conflicto potencial

Authoring:

```text
Origin
  ↓ 10 min
IO
  ↓ 20 min
Strategic
```

Dos interpretaciones posibles:

```text
A. acumulativa:
IO        occurrence +10
Strategic occurrence +30

B. absoluta:
IO        occurrence +10
Strategic occurrence +20
```

Engine espera delays absolutos.

B.1 no congela explícitamente la transformación.

B.2 no está implementado.

Estado:

```text
OPEN / MUST FREEZE BEFORE RESOLUTION IMPLEMENTATION
```

No duplicar un segundo “tiempo de carga” hasta resolver si el recuerdo operacional corresponde a esta misma secuencia.

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

B.2 debe poder producir, desde una misma resolución/provenance:

```text
Runtime materialization
Delivery materialization
```

Runtime necesita:

- executable Rule;
- evaluator reference;
- parameters;
- priority;
- routing;
- management/reappearance inputs;
- adoption semantics.

Delivery necesita:

- display/title/cause;
- color;
- kind/criticality/category/areas;
- Messages;
- deactivation capability;
- visual targets;
- Process projection mode.

Readiness puede diferir:

```text
Runtime READY
Delivery NOT READY
```

si el problema es exclusivamente visual.

Delivery nunca debe liderar al Engine.

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

## 26. OPEN después del rastrillo

### OPEN-1 — Routing tier matrix

Confirmar:

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC
```

y:

- backward prohibited;
- direct PROCESS -> STRATEGIC allowed o no;
- IO origin permitido;
- Strategic origin permitido;
- relación con C1/C2/C3.

### OPEN-2 — Escalation wait semantics

Congelar transformación:

```text
wait_minutes_from_previous_step
->
RoutingDestination.delay_seconds
```

especialmente con 2+ steps.

### OPEN-3 — Rule area vs Tool scope

Congelar compatibilidad:

```text
Rule MINE / PLANT / BOTH
vs
PROCESS operational_scope
vs
Integrated component.scope
```

separando routing de visual projection.

### OPEN-4 — Multiple Message overrides

Definir el contexto operacional exacto cuando una Rule tiene múltiples Messages seleccionados con deactivation overrides diferentes.

### OPEN-5 — Special Conditions runtime

Resolver explícitamente conflicto:

```text
B.1 Special Condition model
vs
Engine IMPACT cascade model
```

incluyendo:

- cascade source;
- cascade target set;
- predominance;
- reappearance por Special Condition.

### OPEN-6 — Reappearance materialization

Definir:

```text
AlarmDefinition.after_minutes
-> runtime due resolver

special_conditions
-> runtime trigger

config change during active ManagementEffect
-> reconciliation
```

### OPEN-7 — Origin Tool adoption

Resolver el conflicto:

```text
B.1: STRUCTURAL_RESET
Engine C2 routing mutation: COMPATIBLE
```

### OPEN-8 — Effective deactivation capability

Definir B.2/Delivery mapping:

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

sin duplicar lifecycle logic en Web.

### OPEN-9 — Routing references vs visual references

Refinar `AlarmToolReferenceReader` para no perder scope y para no usar una única lista que excluye Strategic por razones exclusivamente visuales.

### OPEN-10 — B.2 concrete contract

Definir:

```text
ResolvedAlarmConfiguration
resolution identity/provenance
Runtime readiness
Delivery readiness
findings
materialization boundaries
```

sin resurrectar el strict external pre-save gate superseded.

## 27. Matriz de estado consolidada

| Tema | Estado |
|---|---|
| AlarmConfiguration aggregate | CURRENT / VERIFIED |
| AlarmDefinition B.1 shape | CURRENT / IMPLEMENTED |
| MessageDefinition | CURRENT / IMPLEMENTED |
| Family por `family_key` | CURRENT |
| FamilyDefinition durable | NOT PRESENT / NOT NEEDED YET |
| Priority Group | CURRENT / ENGINE ALIGNED |
| IMPACT-before-RISK invariant | CURRENT / ENGINE ALIGNED |
| Evaluator key + parameters | CURRENT / ENGINE ALIGNED |
| Generic parameter metadata | NOT PRESENT |
| C1 immediate routing | CURRENT / TESTED |
| C2 delayed routing | CURRENT / TESTED |
| C3 origin only | CURRENT / TESTED |
| C2 routing mutation | CURRENT / COMPATIBLE |
| C1/C3 routing mutation | CURRENT / REJECTED |
| Criticality mutation | CURRENT / STRUCTURAL_RESET |
| Kind mutation | CURRENT / REJECTED; B.1 desired differs |
| Evaluator mutation | CURRENT / REJECTED; B.1 desired differs |
| Priority group mutation | CURRENT / REJECTED; B.1 desired differs |
| Origin Tool mutation | CONFLICT / OPEN |
| Special Condition authoring | CURRENT |
| Special Condition runtime semantics | CONFLICT / OPEN |
| Timer reappearance engine | CURRENT |
| AlarmDefinition -> timer mapping | OPEN |
| Special-condition reappearance | OPEN |
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
| Routing tier matrix | UNVERIFIED |
| Alarm Tool Reference scope preservation | GAP |
| Routing/visual reference separation | PROPOSED |
| B.2 | PLANNED |
| Engine qualification baseline | CLOSED PASS/GREEN |
| Engine changes for UI convenience | BLOCKED |

## 28. Invariantes congeladas para el siguiente trabajo

Hasta decisión explícita en contrario:

```text
AlarmDefinition es el contrato editable.

AlarmConfiguration persiste Rules + Messages atómicamente.

VALID != FULLY RESOLVED != READY.

AlarmIdentity = family_key + alarm_key.

alarm_key es identidad estable.

rule_name y display_name son distintos.

Family no equivale a priority_group.

priority_group es el scope de lifecycle/priority.

Existe una sola secuencia priority_order por group.

IMPACT debe preceder RISK.

Parameters son str | float | bool.

Evaluator registry CURRENT resuelve por family_key + evaluator_key.

Messages se seleccionan explícitamente por Rule.

Una Rule sólo usa GLOBAL + misma FAMILY.

Message override reemplaza completamente default deactivation.

Management no detiene routing.

C1 es inmediato.

C2 usa destinos retardados.

C3 es origin only.

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
```

## 29. Conflictos que canonical debe mantener visibles

No resolver silenciosamente:

```text
1. B.1 Special Condition desired semantics
   vs
   Engine CURRENT IMPACT cascade.

2. B.1 reappearance.special_conditions
   vs
   ausencia en PlannedAlarm CURRENT.

3. B.1 origin_tool_key STRUCTURAL_RESET
   vs
   Engine CURRENT C2 routing mutation compatible.

4. B.1 desired compatible kind/evaluator changes
   vs
   Engine CURRENT REJECTED.

5. B.1 desired priority_group migration
   vs
   Engine CURRENT REJECTED.

6. B.2 Increment 2 strict external pre-save gate
   vs
   canonical CURRENT intrinsic-valid persistence.
```

## 30. Próximo foco recomendado

Antes de dibujar la UI definitiva, cerrar un único frente:

```text
ALARM-CONFIGURATION-RESOLUTION-RULES-CLOSURE
```

Objetivo:

```text
resolver sólo los OPEN contractuales que afectan authoring:

- routing tier matrix;
- escalation wait semantics;
- area/scope compatibility;
- multiple Message override semantics;
- Special Condition materialization boundary;
- deactivation effective capability boundary.
```

No implementar B.2 completo todavía.

No modificar Engine todavía.

Una vez cerrados estos puntos:

```text
Authoring model
-> suficientemente estable

UI information architecture
-> puede congelarse

B.2
-> puede diseñarse desde contratos conocidos

Engine
-> sólo se toca si persiste un gap real después de resolution.
```
