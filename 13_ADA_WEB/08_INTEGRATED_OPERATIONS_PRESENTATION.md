# ADA Web — Integrated Operations Presentation

Estado: **CURRENT DESIGN CONTRACT / IMPLEMENTATION PLANNED**

## Propósito

Este documento define la frontera de presentación de ADA Integrated Operations sobre los contratos Web CURRENT.

No define un nuevo runtime, no reemplaza ADA Generic y no transfiere ownership desde Alarm Engine, Tool Configuration, KPI Runtime ni otros dominios.

## Authority

La realidad implementada CURRENT sigue siendo:

```text
moragaga/atlanticus:main
```

La documentación vigente sigue siendo:

```text
moragaga/atlanticus-cannonical:main
```

Referencias de UI/comportamiento:

```text
moragaga/isolated-web-functions:main
    Dash components
    visual/evaluation fixtures
    Operational Trace
    responsive behavior

moragaga/atlanticus-multi-stage:main
    Integrated Operations layout
    overview / mine / plant presentation
    scoped header behavior
    later functional composition
```

Estas referencias no transfieren automáticamente contratos, namespaces, ownership ni arquitectura a Atlanticus CURRENT.

El mock HTML histórico `ada_v164_responsive_4256.html` está **SUPERSEDED como referencia de UI**. Puede conservarse solamente como fuente histórica de datos o ejemplos cuando sea útil.

## Ownership CURRENT

ADA Generic permanece como composition root del producto Web.

CURRENT ya soporta composición externa mediante:

```text
Tool Projection
    ↓
ToolStructure
    ↓
OperationalRenderBinding
    ↓
external composition factory
    ↓
AdaApplicationComposition
    ↓
operational body factory
```

Integrated Operations debe construirse como composición/producto específico sobre esa frontera.

No copiar:

```text
ADA Generic bootstrap
Manager
identity
Navigation runtime
Tool resolution
KPI Collector
application lifecycle
```

No crear un segundo Manager, un segundo bootstrap ni un fork permanente de ADA Generic.

## Tool structural input

Integrated Operations consume el contrato CURRENT:

```text
ToolStructure(kind=INTEGRATED_OPERATIONS)
```

Los componentes operacionales mantienen su `ToolScope` y el orden definido por ToolStructure.

No reintroducir `ToolManifest` ni contratos históricos equivalentes.

Los subcomponentes compartidos deben resolverse mediante los contratos CURRENT de ownership/linking; no duplicar contenido para fabricar geometría de UI.

## Presentation state

La presentación específica de Integrated Operations posee un único estado visual:

```text
overview
mine
plant
```

Este estado es de presentación.

No modifica:

```text
ToolStructure
Tool Configuration
KPI definitions
KPI Collector
Alarm lifecycle
persisted business state
```

No mantener estados independientes que puedan divergir, por ejemplo:

```text
body=mine
header=plant
alarms=overview
```

Una sola presentación debe coordinar todas las superficies visuales que participen.

## Focus semantics

`mine` y `plant` representan **operational focus**, no un zoom geométrico global.

El objetivo del focus es aumentar la superficie útil del área seleccionada mediante:

```text
scope filtering
layout reflow
presentation density changes
available-height recovery
component-aware sizing
```

No implementar el focus aplicando `transform: scale(...)` a la herramienta completa.

El incremento de ancho por sí solo no es objetivo suficiente. La presentación debe aprovechar también el espacio vertical cuando sea posible porque muchos componentes ADA ganan más valor al disponer de mayor altura para tendencias, stockpiles, diagramas, series temporales y otros contenidos operacionales.

## Global Indicators

Los Global Indicators CURRENT se reutilizan; no recuperar las implementaciones históricas como nuevo owner.

La aplicabilidad de un indicador a Integrated Operations es **multi-scope**.

El contrato conceptual debe permitir:

```text
{MINE}
{PLANT}
{MINE, PLANT}
```

Un indicador compartido entre Mina y Planta permanece visible en ambos modos focales.

La asociación entre un indicador y sus scopes pertenece a placement/composition y no debe hardcodearse por `indicator.key` dentro de JavaScript, CSS o renderers específicos.

No es requisito congelado que el tipo implementado se llame `GlobalIndicatorPlacement`; sí queda congelada la separación conceptual:

```text
indicator definition/state
        ≠
scope placement
```

Comportamiento:

```text
overview
    → todos los indicadores aplicables

mine
    → indicadores cuyo placement incluye MINE

plant
    → indicadores cuyo placement incluye PLANT
```

## Header

ADA Operational Shell CURRENT continúa siendo owner del header.

Integrated Operations puede hacer variar la presentación de contenido ya poseído por el header, pero no crea un segundo header.

En focus:

```text
mine
    → Global Indicators aplicables a MINE
    → Management Summary del área Mina

plant
    → Global Indicators aplicables a PLANT
    → Management Summary del área Planta
```

El estado `overview` conserva el contexto global.

## Operational body

El body específico de Integrated Operations se materializa desde `OperationalRenderBinding`.

Responsabilidades de Integrated Operations:

```text
Mina / Planta geometry
scope grouping
component placement
focus controls
focus reflow
tool-specific renderers
tool-specific assets
```

Responsabilidades que permanecen fuera:

```text
Generic runtime
Tool projection ownership
KPI backend
Alarm domain rules
Manager
identity
Navigation ownership
```

Los componentes deben ser materializados respetando el orden y las identidades CURRENT.

## Component presentation

Las capabilities CURRENT tienen prioridad sobre implementaciones históricas cuando ya existe un sucesor.

Clasificación:

```text
Global Indicators        KEEP CURRENT
Time Status              KEEP CURRENT
Alarm Management Summary KEEP CURRENT
Alarm Status             KEEP CURRENT
Card Display             KEEP CURRENT
Content State            KEEP CURRENT
Display Status           KEEP CURRENT

Integrated Ops layout    ADAPT
focus behavior           ADAPT
Stockpile                ADAPT
Metrics                  ADAPT
Wrapped Image            ADAPT
Point Card               ADAPT / rewrite implementation

ToolManifest             SUPERSEDED
component-container      SUPERSEDED
component-card           SUPERSEDED by CURRENT Card Display boundary
state-wrapper            SUPERSEDED by CURRENT Content State boundary
old HTML mock            DATA ONLY / UI SUPERSEDED
```

`ADAPT` significa conservar comportamiento o conocimiento visual útil sin copiar contratos históricos incompatibles con CURRENT.

## Responsive contract

Responsive y operational focus son ejes diferentes.

Responsive responde a la superficie disponible.

Operational focus responde al contexto que el operador quiere observar.

No inferir automáticamente uno desde el otro.

La familia histórica observada incluye:

```text
350
480
1280
1366
1536
1920
2560
```

y existe comportamiento histórico validado para múltiples tamaños/zooms de workstation.

Esto es evidencia de diseño y referencia de continuidad, no autorización para congelar una nueva regla solamente por el número del breakpoint.

La implementación nueva debe recuperar y calificar el comportamiento ya probado antes de inventar reemplazos.

## Notebook and desktop

Notebook y desktop son los principales contextos interactivos para operational focus.

Dirección CURRENT de diseño:

```text
initial presentation → overview
operator may select  → mine | plant
```

El focus debe redistribuir el layout, no solamente ampliar horizontalmente los mismos componentes.

## Tablet

**OPEN / PROPOSED**

Es probable que tablet use una presentación focal como experiencia inicial y navegación Mina ↔ Planta en lugar de comprimir el overview completo.

No queda congelado todavía:

```text
initial tablet presentation
exact tablet breakpoint/classification
availability of overview on tablet
```

Debe calificarse visualmente contra las referencias ya probadas.

## Videowall

Videowall es una superficie de primera clase y está orientada a visión situacional continua.

Dirección congelada:

```text
videowall → overview
videowall → no operational focus controls
```

No usar focus Mina/Planta como mecanismo normal de videowall.

La superficie adicional debe aprovecharse para legibilidad y contenido simultáneo, por ejemplo:

```text
larger relevant values
larger charts
larger industrial visuals
larger stockpiles
clearer alarm routes/baseline
distance-readable hierarchy
```

No escalar uniformemente toda la aplicación.

**OPEN:** no identificar videowall solamente mediante `width >= 2560px`. Resolución, tamaño físico, aspect ratio o un modo explícito pueden requerir una regla distinta. La detección final debe demostrarse antes de congelarse.

## Alarm presentation boundary

Alarm Engine y Command Center conservan ownership separado.

Integrated Operations no reimplementa:

```text
priority
lifecycle
management eligibility
deactivation rules
routing business rules
capabilities
```

La UI puede adaptar densidad y geometría sobre una proyección vigente.

En focus existe la dirección de reducir costo vertical de la superficie de alarmas, potencialmente mediante una presentación compacta tipo pill.

Estado:

```text
compact alarm presentation / pills → PROPOSED
exact pill contract                → OPEN
Alarm Engine integration           → SEPARATE FOCUS
```

No congelar contenido, acciones o filtering de pills antes de disponer del contrato vigente de proyección de alarmas.

El filtering visual por área debe usar información proveniente del contrato de alarma; no inferir área desde nombres, posición o claves hardcodeadas.

## Alarm baseline

La implementación CURRENT de Alarm Baseline Projection/Surface tiene prioridad sobre Operational Trace histórico.

Operational Trace es:

```text
SUPERSEDED como arquitectura monolítica
REFERENCE para comportamiento/geometry/interaction
```

El comportamiento futuro puede recuperar conocimiento probado de rutas, selección, placement y visualización sin devolver ownership de dominio a UI.

## Responsive implementation principles

Preferir:

```text
CSS grid/flex reflow
container-aware component sizing
presentation filtering
density variants
responsive assets
component-specific scaling where justified
```

Evitar:

```text
global application scale transforms
hardcoded KPI names for layout
duplicated Mina/Planta business state
viewport-size business logic in backend
separate runtime per presentation mode
```

## Visual qualification

Responsive, spacing, density, videowall legibility y appearance se califican visualmente.

No crear tests cuyo único objetivo sea congelar:

```text
exact CSS geometry
pixel spacing
specific visual breakpoint layout
presence/absence of implementation classes
```

Los tests automatizados deben concentrarse en comportamiento verificable, por ejemplo:

```text
valid presentation states
scope filtering semantics
shared indicator visibility
component materialization
renderer coverage
presentation state transitions
asset loading where contractually relevant
no business-state mutation from focus
```

## Implementation sequence

PLANNED:

```text
1. define CURRENT scope-placement contract for Global Indicators
2. create Integrated Operations external composition over ADA Generic
3. materialize MINE / PLANT operational body from OperationalRenderBinding
4. implement overview / mine / plant coordinated presentation
5. recover and qualify responsive workstation behavior
6. qualify videowall overview behavior
7. adapt real process components incrementally
8. integrate compact Alarm presentation only against the current Alarm projection contract
```

No mezclar el primer incremento con rediseño de Alarm Engine, Command Center o KPI backend.

## Open items

OPEN:

```text
exact implementation type/name for Global Indicator placement
complete inventory of historically validated workstation zoom/responsive rules
tablet initial presentation
tablet breakpoint/classification
videowall detection rule
exact focus geometry at each qualified viewport
compact Alarm pill contract
final Alarm focus filtering against current projection
```

Estos puntos no deben completarse silenciosamente durante implementación.
