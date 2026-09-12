# Manager — Tool Configuration

Estado: **FROZEN/CURRENT**

## Autoridad estructural

Tool Configuration determina qué estructura existe.

Data determina el estado de lo que ya existe.

Una Tool correctamente configurada debe poder montar su UI aunque todavía no existan datos.

## Tool kinds

Baseline operacional congelado:

### PROCESS
- ámbito operacional global;
- Components con `layout_role`;
- CENTER obligatorio;
- baseline de alarmas centrado en operación central.

### INTEGRATED OPERATIONS
- sin ámbito global único;
- cada Component declara scope;
- baseline de alarmas considera todos los Components.

## Topología

Component/Subcomponent keys son identidad consumible.

No son sólo etiquetas visuales.

Se utilizan como frontera para:
- datos;
- KPI;
- render;
- routing visual;
- alarmas.

## Component

Component es la unidad funcional de datos:

- 1 Store;
- 1 Collector contract;
- destino KPI;
- baseline/ámbito de alarmas;
- puede contener N Subcomponents.

## Subcomponent

Subcomponent es granularidad visual interna:

- no Store propio;
- no Collector propio;
- no destino KPI;
- sí puede ser target visual independiente de alarma.

## Regla

`N Subcomponents != N Stores != N Collectors`

No fragmentar un Component en infraestructura adicional sin escala real que lo justifique.

## Render vacío

Cosmos/Store vacío es un estado válido.

- configurado + sin dato → `EMPTY`;
- expectativa que no puede resolverse → `NOT_MAPPED`;
- no pertenece a Tool → no render.

## Alarmas

Estado de datos y estado de alarmas son dimensiones independientes.

Un Subcomponent puede:
- data = EMPTY;
- alarm = CRITICAL.

La alarma no determina existencia estructural.
