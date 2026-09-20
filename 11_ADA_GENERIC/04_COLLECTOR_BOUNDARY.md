# ADA Generic — Collector Boundary

Estado: **SEMANTICS FROZEN / IMPLEMENTATION BLOCKED BY KPI BACKEND FLOW**

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

```text
N Subcomponents != N Stores != N Collectors
```

## Collector no equivale automáticamente a Producer

No renombrar Producer a Collector.

No crear un wheel `collectors` por coincidencia terminológica.

No duplicar sources/materializers ya válidos.

ADA Generic no debe conocer productores concretos.

## Gate CURRENT

La implementación del Collector queda bloqueada hasta cerrar:

```text
KPI-RUNTIME-REPROCESS-CURRENT
KPI-DELIVERY-REGISTRY-CONSUMPTION
KPI-TIMESERIES-REGISTRY-CONSUMPTION
KPI-HISTORIAN-REPROCESS-CURRENT
```

Razón:

la frontera final del flujo KPI debe estar calificada antes de fijar el mapping físico del Collector.

## Después del gate

Para la primera Tool/Component real:

```text
1. identificar Source operacional
2. identificar materialización Store/dataset
3. identificar scheduling/runtime
4. identificar entrada/salida KPI final
5. contrastar con Component contract
6. introducir sólo el gap mínimo real
```

Posibles resultados:

```text
Producer existente cumple Collector contract
Producer alimenta Collector delgado
varios Producers alimentan un Collector
Collector es sólo contrato/configuración
falta una responsabilidad nueva
```

No elegir antes de mapear la vertical real.

## Estado

```text
ADA-GENERIC-COLLECTOR-CLOSURE
BLOCKED / AFTER KPI BACKEND FLOW
```
