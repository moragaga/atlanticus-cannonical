# ADA Command Center — Alarm Configuration Authoring Model

Estado: **DRAFT / IN PROGRESS**

Propósito: **guía canónica de trabajo para reconstruir y completar el modelo de authoring de Alarm Configuration antes de rediseñar la UI o proponer cambios al Alarm Engine.**

Este documento **no congela decisiones nuevas**. Separa explícitamente lo ya verificado de lo que todavía debe confirmarse con decisiones, implementación histórica u otras fuentes que se incorporen después.

## 1. Autoridad y evidencia usada

Jerarquía de autoridad aplicada:

1. `moragaga/atlanticus:main`: realidad implementada.
2. Decisiones explícitamente vigentes/frozen en `moragaga/atlanticus-decisions:main`.
3. Qualification y tests.
4. Decisiones recientes todavía no formalizadas.
5. Recuerdo conversacional: pista para búsqueda, nunca autoridad suficiente por sí sola.

Implementación auditada para esta guía:

```text
moragaga/atlanticus:main
762c8db89d8811b2036a84e4c82904e6ee31ec28
```

Fuentes principales consultadas:

```text
atlanticus-decisions:
alarm_decisions/R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md

atlanticus-canonical:
00_AUTHORITY.md
04_ALARM_ENGINE__01_DOMAIN_MODEL.md

atlanticus:
scopes/ada-command-center/backend/alarms/core/
scopes/ada-command-center/backend/tools/catalog/
scopes/ada-command-center/web/alarms/configuration/
scopes/ada/web/tools/core/
web/capabilities/manager/
```

## 2. Objetivo de esta guía

La configuración de alarmas no debe tratarse como un formulario plano.

El problema de authoring incluye simultáneamente:

- familias;
- Rules;
- prioridad y `priority_group`;
- Special Conditions;
- evaluadores y parámetros;
- mensajes reutilizables;
- management y reappearance;
- desactivación;
- routing/escalamiento entre Tools;
- áreas operacionales Mine/Plant;
- visual targets;
- Components y Subcomponents;
- semántica específica de Process;
- resolución posterior hacia `PlannedAlarm`.

La UI futura debe representar esas relaciones sin cambiar innecesariamente el contrato durable ni introducir lógica que pertenece a B.2 o al Runtime.

## 3. Frontera general

**VERIFIED / CURRENT**

La cadena conceptual vigente es:

```text
Alarm Configuration
        |
        v
AlarmDefinition
        |
        v
B.2 / Configuration Resolution
        |
        +--> referencias externas
        +--> Tool Configuration
        +--> Message Catalog
        +--> evaluator registry
        |
        v
PlannedAlarm + AlarmExecutionEntry
        |
        v
Alarm Runtime
```

`AlarmDefinition` es configuración editable.

`PlannedAlarm` es la forma resuelta y ejecutable.

El Core no debe conocer Dash/Flask, geometría UI, Cosmos, SharePoint, sesiones web ni persistencia física.

Consecuencia para este trabajo:

> La UI debe facilitar la edición del contrato canónico y sus referencias. No debe absorber responsabilidades propias del resolver ni modificar el Engine sólo para simplificar el formulario.

## 4. Conceptos base

### 4.1 Rule

**VERIFIED**

Una Rule es una alarma configurada.

Su contrato editable es `AlarmDefinition`.

Una activación concreta en Runtime es una `Occurrence`.

### 4.2 Family

**VERIFIED**

La identidad de una Rule contiene:

```python
AlarmIdentity(
    family_key: str,
    alarm_key: str,
)
```

`family_key` agrupa lógicamente Rules relacionadas.

Actualmente no existe un `FamilyDefinition` durable separado en `AlarmConfiguration`.

**PROPOSED**

Para authoring, la Family debe utilizarse como **agregado visual derivado** de los `family_key` existentes, sin introducir todavía un nuevo contrato durable.

Conceptualmente:

```text
Family
|
+-- Rules
|   |
|   +-- Priority Group A
|   +-- Priority Group B
|
+-- Family Messages
|
+-- Global Messages disponibles
```

### 4.3 Priority Group

**VERIFIED**

`priority_group` define el contexto donde Rules relacionadas:

- participan del mismo lifecycle/episode;
- compiten por prioridad;
- interactúan con Special Conditions.

Existe una única secuencia `priority_order` dentro de cada `priority_group`.

### 4.4 Message

**VERIFIED**

Los Messages son contratos reutilizables.

Existen dos scopes:

```text
GLOBAL
FAMILY
```

`GLOBAL` no es una Family.

### 4.5 Tool

**VERIFIED**

Los tipos actuales de Tool Configuration son:

```text
PROCESS
INTEGRATED_OPERATIONS
STRATEGIC
```

Alarm Configuration almacena referencias por keys; Tool Configuration conserva ownership de topología, scopes, Components y Subcomponents.

## 5. Identidad y nombres de una Rule

**VERIFIED**

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
-> familia lógica.

alarm_key
-> identidad estable de la Rule.
-> usada en lifecycle, persistencia y referencias.

rule_name
-> nombre técnico/operativo.
-> editable.
-> único dentro de la Family.

display_name
-> friendly name humano.

title
-> título estático mostrado por la alarma.

cause_template
-> texto dinámico que puede incorporar evidence/evaluation.
```

No deben colapsarse estos campos sólo porque hoy parezcan similares.

**PROPOSED**

En una UI:

- `alarm_key` debe presentarse como identidad estable y no como un texto ordinario de edición casual;
- `rule_name` debe explicarse como nombre técnico/operativo;
- `display_name` como friendly name;
- `title` y `cause_template` como contenido operacional.

## 6. Clasificación y estado de la Rule

**VERIFIED**

Campos actuales:

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

Catálogos:

```text
visibility_mode:
- VISIBLE
- TRACE_ONLY

kind:
- RISK
- IMPACT

criticality:
- C1
- C2
- C3

business_category:
- ECOLOGY
- PRODUCTIVITY
- SAFETY_HEALTH
- COSTS

operational_areas:
- MINE
- PLANT
- una o ambas, sin valor artificial BOTH

color:
- RED
- YELLOW
```

`TRACE_ONLY` continúa evaluándose y trazándose, pero no debe proyectarse como alarma visible.

Una Rule `is_active=false` continúa definida, pero queda fuera del flujo ejecutable.

## 7. Evaluación y parámetros

**VERIFIED**

Cada Rule declara:

```text
evaluator_key
parameters
```

Contrato de `parameters`:

```python
Mapping[str, str | float | bool]
```

Permitidos:

```text
TEXT
FLOAT
BOOLEAN
```

No permitidos:

```text
None
listas
tuplas como valor
dict anidado
código
expresiones
```

Un entero numérico del negocio se representa contractualmente como `float`.

Ejemplo:

```text
120.0
```

y no como un tipo `int` de dominio.

La semántica de cada parameter pertenece al evaluator/desarrollador.

**CURRENT IMPLEMENTATION NOTE**

El authoring ya normaliza el round-trip JSON/browser donde un `1200.0` puede volver como `1200`, preservando finalmente el contrato `float`.

**PROPOSED FOR UI**

No usar un único campo `Parameters JSON`.

Representar los parámetros dinámicamente:

```text
Key                 Type       Value
threshold_tph       Number     1200.0
window              Text       15m
quality_required    Boolean    Yes
```

con operaciones:

```text
+ Add parameter
Remove
```

Esto no requiere todavía un catálogo de evaluator schemas.

**OPEN**

Si posteriormente existe metadata contractual del evaluator, la UI puede aprovecharla para nombres, tipos y validaciones más específicas sin cambiar el shape durable.

## 8. Priority y Special Conditions

### 8.1 Una sola secuencia

**VERIFIED**

Dentro de cada `priority_group`:

```text
priority_order > 0
priority_order único
```

Rules normales y Special Conditions comparten la misma secuencia.

Ejemplo válido:

```text
Priority Group: CRUSHING

1 -> Special Condition A
2 -> Rule B
3 -> Special Condition C
4 -> Rule D
```

### 8.2 IMPACT y RISK

**VERIFIED**

La revisión completa mantiene la invariante de prioridad:

```text
IMPACT debe preceder RISK dentro del mismo priority_group
```

### 8.3 Special Condition

**VERIFIED**

Una Special Condition no es otro `AlarmKind`.

Es una Rule normal con:

```text
is_special_condition = true
```

Conserva:

- kind;
- criticality;
- evaluator;
- parameters;
- priority;
- routing;
- management;
- targets.

Una referencia a Special Condition para reappearance debe apuntar a una Rule:

```text
de la misma Family
AND
del mismo priority_group
AND
marcada is_special_condition=true
```

### 8.4 Semántica de gestión

**VERIFIED EN DECISIÓN FROZEN**

Una Special Condition predominante gestionada puede ejercer Special Cascade sobre las demás Rules activas del mismo `priority_group`.

La prioridad de la Special Condition sigue usando el mismo `priority_order`.

**PROPOSED FOR UI**

No pedir `priority_order` principalmente como número.

Representar el Priority Group como una secuencia ordenable:

```text
CRUSHING

1  Crusher Trip                IMPACT · Special
2  Crusher Throughput Risk     RISK
3  Low Stockpile               RISK
```

La UI puede persistir automáticamente `priority_order`.

## 9. Management y reappearance

**VERIFIED**

Contrato:

```python
ReappearanceDefinition(
    after_minutes: int | None,
    special_conditions: tuple[AlarmIdentity, ...],
)
```

No existe `enabled`.

```text
after_minutes=None
special_conditions=()
```

significa que no existe reappearance configurado.

Una occurrence gestionada reaparece cuando:

```text
main condition remains active
AND
(
    after_minutes elapsed
    OR
    any referenced Special Condition is active
)
```

Si la condición principal deja de estar activa, una referencia o timer anterior no debe resucitar esa occurrence.

`after_minutes`:

```text
None
o
> 0
```

sin máximo artificial definido en el contrato.

**VERIFIED**

Mientras Management oculta una alarma, el routing no se detiene automáticamente.

## 10. Deactivation de la Rule

**VERIFIED**

Contrato:

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
-> max_duration_hours requerido
-> 1 <= max_duration_hours <= 12
```

La desactivación es una capacidad ofrecida al operador.

El operador sigue siendo quien decide si desactivar y hasta cuándo dentro del máximo efectivo permitido.

El fin real puede depender además de restricciones Runtime como fin de turno; B.1 no resuelve eso.

### 10.1 Approval

**VERIFIED CONTRACT**

`approval_required` existe en el contrato.

**CURRENT PROJECT CONTEXT**

El flujo operacional completo de aprobación todavía no debe asumirse implementado sólo porque el flag exista.

La UI de configuración puede expresar la intención contractual sin inventar el workflow de aprobación.

## 11. Message Catalog

### 11.1 Organización

**VERIFIED**

Conceptualmente:

```text
Message Catalog
|
+-- GLOBAL
|
+-- FAMILY A
|
+-- FAMILY B
```

Contrato:

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

Reglas:

```text
GLOBAL
-> family_key=None

FAMILY
-> family_key requerido
```

`message_key` es identidad estable.

`display_text` no es identidad.

### 11.2 Selección desde una Rule

**VERIFIED**

Cada Rule declara explícitamente:

```text
message_keys[]
```

Una Rule de `FAMILY_A` puede seleccionar:

```text
GLOBAL
+
FAMILY_A
```

No puede seleccionar Messages de otra Family.

Una Rule puede no tener Messages.

Crear un Message GLOBAL nuevo no modifica Rules existentes automáticamente.

### 11.3 Deactivation override del Message

**VERIFIED**

Un Message puede declarar:

```python
MessageDeactivationDefinition(
    enabled,
    max_duration_hours,
    approval_required,
)
```

Precedencia:

```text
message.deactivation_override is None
-> usar default de la Rule

override existe
-> reemplaza completamente el default de la Rule
```

No se mezclan campos individualmente.

Ejemplo:

```text
Rule max deactivation = 2h
Message override = 7h
-> contexto del Message usa 7h

Rule max deactivation = 2h
Message override = disabled
-> ese contexto no ofrece deactivation
```

**OPEN / NEEDS CONFIRMATION**

Cuando una Rule tiene múltiples Messages seleccionados, debe verificarse cómo se materializa la política efectiva si esos Messages tienen overrides distintos. No asumir una regla de combinación hasta revisar la fuente correspondiente.

## 12. Routing y escalamiento

### 12.1 Contrato editable

**VERIFIED**

Cada Rule declara:

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
origin_tool_key no vacío
step_order > 0
step_order único
target_tool_key no vacío
target != origin
targets sin duplicados
wait is None OR >= 0
```

B.2 debe validar existencia y compatibilidad de Tools.

### 12.2 Criticality y routing

**VERIFIED EN DECISIÓN FROZEN**

Semántica histórica a preservar:

```text
C1
-> destinos inmediatos

C2
-> destinos retardados

C3
-> origin only
```

Los detalles de tipos/tier permitidos pertenecen a resolución contra Tool Configuration.

### 12.3 Jerarquía recordada

**UNVERIFIED / TO CONFIRM**

Se recuerda la jerarquía operacional:

```text
PROCESS
    ↓
INTEGRATED_OPERATIONS
    ↓
STRATEGIC
```

y la restricción:

```text
nunca escalar hacia atrás
```

También se recuerda que:

```text
PROCESS puede escalar a INTEGRATED_OPERATIONS
INTEGRATED_OPERATIONS puede escalar a STRATEGIC
```

Esta matriz debe confirmarse explícitamente antes de congelarse como contrato.

### 12.4 Tiempo de carga / espera por destino

**PARTIALLY VERIFIED**

El contrato CURRENT posee:

```text
wait_minutes_from_previous_step
```

por cada paso de escalamiento.

**UNVERIFIED**

Debe confirmarse si el “tiempo de carga” recordado para cada Tool/destino corresponde exactamente a:

```text
wait_minutes_from_previous_step
```

o si existe otra semántica/configuración histórica independiente.

No duplicar conceptos hasta resolver esta pregunta.

## 13. Tool scopes y áreas Mine / Plant

### 13.1 Process

**VERIFIED**

Una Tool `PROCESS` requiere:

```text
operational_scope:
- MINE
- PLANT
```

Por lo tanto una Rule sólo debería recibir como opciones Process compatibles con sus `operational_areas`.

Ejemplo conceptual:

```text
Rule areas = MINE
-> Process MINE disponible
-> Process PLANT no debe ofrecerse como opción compatible
```

### 13.2 Integrated Operations

**VERIFIED**

`INTEGRATED_OPERATIONS` no posee un único `operational_scope`.

Sus Components declaran individualmente:

```text
scope = MINE | PLANT
```

Por eso puede integrar ambos dominios.

### 13.3 Strategic

**VERIFIED**

`STRATEGIC` existe como `ToolConfigurationKind`.

**VERIFIED**

Todavía no existe contrato suficiente para proyección visual específica de alarmas Strategic.

**OPEN**

Routing hacia Strategic y visual projection de Strategic deben tratarse como responsabilidades distintas.

Strategic puede terminar siendo válido como destino de routing sin que eso implique que ya exista un contrato visual.

## 14. Gap actual en Alarm Tool References

**VERIFIED / CURRENT**

`AlarmToolReferenceReader` actualmente entrega:

```text
tool_key
display_name
kind
source_release_id
components
subcomponents
```

Además actualmente omite `STRATEGIC` del catálogo entregado a Alarm Configuration.

También pierde información necesaria para filtrado operacional:

```text
PROCESS operational_scope
INTEGRATED_OPERATIONS component.scope
```

### Consecuencia

La UI CURRENT no puede realizar correctamente por sí sola:

```text
Rule MINE
-> mostrar sólo Process MINE

Rule PLANT
-> mostrar sólo Process PLANT
```

sin duplicar o inventar conocimiento.

**PROPOSED**

Antes de una UI final, refinar el contrato de referencias para preservar el scope necesario.

También evaluar separar conceptualmente:

```text
Routing references
```

de:

```text
Visual projection references
```

porque Strategic puede tener reglas diferentes entre ambas superficies.

## 15. Visual Targets

**VERIFIED**

Cada Rule puede declarar múltiples:

```python
AlarmVisualTarget(
    tool_key,
    component_keys,
    subcomponents,
    process_projection_mode,
)
```

### 15.1 Integrated Operations

**VERIFIED**

Semántica:

```text
Component
-> dónde se posiciona/organiza la alarma

Subcomponents
-> elementos que reciben color/afectación visual
```

Una Rule puede afectar múltiples Components y Subcomponents.

### 15.2 Identidad del Subcomponent

**VERIFIED**

La identidad durable es:

```text
(owner_component_key, subcomponent_key)
```

Esto es importante porque Integrated Operations puede hacer visible un Subcomponent desde otro Component mediante links, pero debe conservarse el owner real.

### 15.3 Process

**VERIFIED**

Para un target `PROCESS`, cada Rule declara:

```text
GENERIC
o
DISTRIBUTED
```

mediante:

```text
process_projection_mode
```

La geometría concreta del panel Process no pertenece a Alarm Core.

### 15.4 Strategic

**VERIFIED**

No inventar `process_projection_mode` ni comportamiento visual Strategic mientras no exista evidencia contractual.

## 16. Estructura durable actual de AlarmDefinition

**VERIFIED**

```text
AlarmDefinition
|
+-- identity
|   +-- family_key
|   +-- alarm_key
|
+-- rule_name
+-- display_name
+-- title
+-- cause_template
|
+-- is_active
+-- visibility_mode
+-- is_special_condition
|
+-- kind
+-- criticality
+-- business_category
+-- operational_areas[]
+-- color
|
+-- evaluator_key
+-- parameters{}
|
+-- priority_group
+-- priority_order
|
+-- message_keys[]
|
+-- reappearance
|   +-- after_minutes
|   +-- special_conditions[]
|
+-- default_deactivation
|   +-- enabled
|   +-- max_duration_hours
|   +-- approval_required
|
+-- escalation
|   +-- origin_tool_key
|   +-- steps[]
|       +-- step_order
|       +-- target_tool_key
|       +-- is_enabled
|       +-- wait_minutes_from_previous_step
|
+-- visual_targets[]
    +-- tool_key
    +-- component_keys[]
    +-- subcomponents[]
    |   +-- owner_component_key
    |   +-- subcomponent_key
    +-- process_projection_mode
```

## 17. Estructura durable actual de MessageDefinition

**VERIFIED**

```text
MessageDefinition
|
+-- message_key
+-- scope
|   +-- GLOBAL
|   +-- FAMILY
|
+-- family_key | None
+-- display_text
+-- is_active
|
+-- deactivation_override | None
    +-- enabled
    +-- max_duration_hours
    +-- approval_required
```

## 18. Modelo de authoring propuesto

Esta sección describe una forma de entender la configuración. **No es todavía un contrato UI congelado.**

**PROPOSED**

```text
Alarm Configuration
|
+-- Global Messages
|
+-- Families
    |
    +-- Family A
        |
        +-- Family Messages
        |
        +-- Priority Groups
        |   |
        |   +-- Group 1
        |       |
        |       +-- Rule / Special Condition
        |       +-- Rule
        |       +-- Rule
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

La Family sería una vista derivada, no necesariamente una nueva entidad persistida.

## 19. Implicaciones para una futura UI

**PROPOSED / NOT FROZEN**

La UI debería evitar:

- todas las Rules expandidas simultáneamente;
- JSON manual para parameters;
- `priority_order` como único mecanismo de orden;
- Tools/Components/Subcomponents como campos independientes sin jerarquía;
- mostrar keys técnicas cuando existe `display_name`;
- mezclar routing con visual projection;
- obligar al usuario a conocer compatibilidad Mine/Plant manualmente.

Direcciones a evaluar después de completar esta guía:

```text
Family-first navigation
Master/detail para Rules
Master/detail para Messages
Priority Groups ordenables
Parameters tipados dinámicos
Message selection GLOBAL + FAMILY
Routing como secuencia dirigida
Visual targets como árbol Tool -> Component -> Subcomponent
Filtrado por operational area
Progressive disclosure de campos dependientes
```

## 20. Relación con el Engine

### 20.1 Principio de trabajo

**CURRENT PROJECT DIRECTION**

El Alarm Engine ya fue sometido a un ciclo largo de pruebas y se considera operacionalmente estable para los escenarios probados.

Por tanto:

> No modificar el Engine por conveniencia de authoring.

Primero determinar si una necesidad:

```text
A. ya existe en AlarmDefinition;
B. puede resolverse en B.2;
C. pertenece a Tool Configuration;
D. pertenece al evaluator;
E. pertenece a Delivery/UI;
F. es realmente un gap del Engine.
```

Sólo el último caso justifica proponer evolución del Engine.

### 20.2 Deltas históricos que deben revalidarse

**VERIFIED EN DECISIÓN FROZEN, CURRENT IMPLEMENTATION STATUS TO RECHECK**

La decisión B.1 documentó como delta respecto del engine de ese momento:

```text
evaluator_key
CURRENT histórico: REJECTED
TARGET: COMPATIBLE

kind
CURRENT histórico: REJECTED
TARGET: COMPATIBLE

priority_group
CURRENT histórico: REJECTED
TARGET: migración estructural entre grupos

origin_tool_key
TARGET: STRUCTURAL_RESET
```

Antes de modificar Engine por cualquiera de estos puntos se debe auditar el estado actual de `main`; no asumir que el delta histórico sigue abierto.

## 21. OPEN — investigación requerida antes de congelar UX

Los siguientes puntos quedan explícitamente abiertos:

1. **Matriz de routing por Tool tier**
   - confirmar `PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC`;
   - confirmar prohibición estricta de escalamiento hacia atrás;
   - confirmar si existen saltos permitidos, por ejemplo `PROCESS -> STRATEGIC`.

2. **Criticality y routing**
   - confirmar reglas completas de C1/C2/C3 contra tiers;
   - confirmar si C1 usa steps con wait=0 o si B.2 materializa destinos inmediatos de otra forma.

3. **“Tiempo de carga” por Tool/destino**
   - confirmar si equivale a `wait_minutes_from_previous_step`;
   - confirmar si existe un segundo concepto histórico.

4. **Strategic**
   - confirmar participación en routing;
   - mantener comportamiento visual como pendiente hasta nueva evidencia.

5. **Area compatibility**
   - confirmar reglas exactas cuando una Rule tiene `(MINE, PLANT)`;
   - confirmar cómo aplica scope a routing y a visual targets por separado.

6. **Multiple Messages con overrides**
   - confirmar cómo se materializa la política de deactivation cuando una Rule utiliza varios Messages con overrides distintos.

7. **Evaluator metadata**
   - determinar si existe o existirá un catálogo de evaluator/parameter definitions;
   - no bloquear UI V1 por este punto.

8. **Approval**
   - mantener `approval_required` como configuración;
   - no inventar workflow operativo mientras no exista contrato.

9. **Family authoring**
   - confirmar si Family seguirá siendo únicamente derivada de `family_key`;
   - no introducir `FamilyDefinition` sin una necesidad contractual real.

10. **B.2 actual**
    - localizar y auditar la decisión/implementación vigente de Configuration Resolution;
    - contrastar sus invariantes con esta guía.

11. **Engine actual**
    - auditar únicamente después de completar el modelo;
    - comparar contratos antes de proponer modificaciones.

## 22. Estado consolidado

| Elemento | Estado | Evidencia |
|---|---|---|
| AlarmDefinition durable | CURRENT / VERIFIED | Core + B.1 |
| Family por `family_key` | CURRENT / VERIFIED | AlarmIdentity |
| Family como agregado UI | PROPOSED | Derivable, no durable |
| Priority Group único | CURRENT / VERIFIED | B.1 + configuration |
| Special Conditions en misma prioridad | CURRENT / VERIFIED | B.1 |
| Reappearance | CURRENT / VERIFIED | Core + B.1 |
| Rule deactivation | CURRENT / VERIFIED | Core + B.1 |
| Message GLOBAL/FAMILY | CURRENT / VERIFIED | Core + B.1 |
| Message deactivation override | CURRENT / VERIFIED | Core + B.1 |
| Routing origin + steps | CURRENT / VERIFIED | Core + B.1 |
| C1/C2/C3 histórico | VERIFIED / requiere contraste B.2 actual | B.1 |
| PROCESS scope Mine/Plant | CURRENT / VERIFIED | ToolStructure |
| Integrated Operations component scope | CURRENT / VERIFIED | ToolStructure |
| STRATEGIC Tool kind | CURRENT / VERIFIED | Tool enums |
| Strategic visual projection | PLANNED / UNDEFINED | B.1 + ToolStructure |
| Routing ascendente Process -> IO -> Strategic | UNVERIFIED | recuerdo a confirmar |
| No routing hacia atrás | UNVERIFIED | recuerdo a confirmar |
| ToolReference conserva scopes | BLOCKED / GAP | implementación actual no los expone |
| Parameters JSON como UX final | SUPERSEDED / PROPOSED replacement | structured parameters |
| UI final | PLANNED | pendiente completar modelo |
| Cambios al Engine | BLOCKED | primero completar/contrastar contratos |

## 23. Invariantes a conservar durante la investigación

Hasta que nueva evidencia explícita los reemplace:

```text
AlarmDefinition sigue siendo contrato editable canónico.

AlarmIdentity = family_key + alarm_key.

alarm_key es identidad estable.

rule_name y display_name son conceptos distintos.

Una Rule pertenece al menos a MINE o PLANT.

Un priority_group tiene una sola secuencia de priority_order.

Special Conditions participan de esa misma secuencia.

Parameters son str | float | bool.

Messages son explícitamente seleccionados por cada Rule.

Una Rule puede seleccionar GLOBAL + misma FAMILY.

Message override reemplaza completamente default deactivation de la Rule.

Management no detiene routing por sí mismo.

Visual target y routing son responsabilidades diferentes.

Component determina posicionamiento en Integrated Operations.

Subcomponents determinan afectación/color visual.

Subcomponent durable identity = owner_component_key + subcomponent_key.

Process target declara GENERIC o DISTRIBUTED.

Strategic visual behavior no se inventa.

Tool Configuration mantiene ownership de topology y scope.

Alarm Configuration persiste referencias, no copias de Tool Configuration.

No modificar Engine sólo para simplificar authoring.
```

## 24. Próxima etapa de esta guía

Este documento debe enriquecerse con las fuentes adicionales que se identifiquen.

El siguiente ciclo debe:

```text
1. buscar evidencia histórica faltante;
2. completar routing/tier rules;
3. completar semántica de tiempos;
4. revisar B.2;
5. contrastar con Engine CURRENT;
6. cerrar OPEN;
7. recién entonces trazar UX/UI final;
8. implementar cambios incrementales fuera del Engine cuando sea posible.
```

Hasta completar esos pasos, este documento permanece:

```text
DRAFT / IN PROGRESS
```
