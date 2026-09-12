# ADA Generic — Collector Boundary

Estado: **SEMANTICS FROZEN / PHYSICAL MAPPING OPEN**

## Semántica congelada

Component es la frontera funcional de datos de la Tool.

Por Component:

- 1 Store conceptual;
- 1 Collector contract conceptual;
- 0..N Subcomponents;
- destino KPI;
- baseline/ámbito de alarmas.

Subcomponent:

- no Store propio;
- no Collector propio;
- no destino KPI;
- sí puede ser target visual independiente de alarma.

Regla:

`N Subcomponents != N Stores != N Collectors`

## Collector no equivale automáticamente a Producer

Actualmente `main` no contiene una capability top-level llamada literalmente `collectors`.

Sí contiene Operational Data Producers, Sources y Processes.

El Producer auditado de Fabrica compone:

- conexiones Storage;
- `FabricaStorageSource`;
- `FabricaMaterializer`;
- `DatasetRuntime`;
- producer state;
- `FabricaJob`.

Su materializer transforma una fuente operacional y publica datasets mediante `DatasetRuntime`.

Eso prueba que Producer es una capacidad física de ingestión/materialización, pero NO demuestra por sí mismo que sea el Collector contractual de un Component ADA.

El core de producers auditado expone `SourceScopeProvider`; no existe allí todavía un contrato genérico `ComponentCollector`.

## Regla de diseño

No:

- renombrar Producer a Collector;
- crear un nuevo wheel `collectors` por coincidencia terminológica;
- duplicar sources/materializers ya válidos;
- hacer que ADA Generic conozca productores concretos.

Sí:

1. seleccionar una Tool/Component real;
2. identificar su Source operacional;
3. identificar quién materializa su Store/dataset;
4. identificar scheduling/runtime;
5. contrastar eso con el contrato funcional del Component;
6. definir el adaptador/frontera mínima sólo si existe un gap real.

## Posibles resultados del mapping

El barrido puede concluir que:

- un Producer existente cumple directamente el Collector contract;
- un Producer expone datos que un Collector delgado consume;
- varios Producers alimentan un Collector;
- el Collector es sólo un contrato/configuración sobre capacidades existentes;
- falta una responsabilidad nueva.

No elegir una opción antes de mapear una Tool real.

## Consecuencia para el primer entregable

La primera vertical debe utilizar un Component concreto.

El mapping `Component -> Collector contract -> capacidad física de datos` debe quedar explícito y verificable antes de generalizar la solución.
