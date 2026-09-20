# ADA Generic — Collector Boundary

Estado: **PLANNED / NEXT**

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

```text
N Subcomponents != N Stores != N Collectors
```

## Gate KPI

El gate anterior está cerrado:

```text
KPI-RUNTIME-REPROCESS-CURRENT               CLOSED / CURRENT
KPI-DELIVERY-REGISTRY-CONSUMPTION           CLOSED / CURRENT
KPI-TIMESERIES-REGISTRY-CONSUMPTION        CLOSED / CURRENT
KPI-HISTORIAN-REPROCESS-CURRENT             CLOSED / CURRENT
```

Collector deja de estar BLOCKED.

## Inputs CURRENT disponibles

```text
Latest Delivery Cosmos
ada-kpi-latest-delivery
id=latest
partition_id=kpis

Timeseries Delivery Cosmos
ada-kpi-timeseries-delivery
id=timeseries
partition_id=kpis

Tool CURRENT contract
component/destination structure
```

## Decisión de scheduling

```text
Latest y Timeseries deben tener intervalos de carga distintos.
Latest es prioritario.
```

No se fijan valores numéricos en este cierre.

## OPEN para el siguiente chat

Inspeccionar antes de diseñar código:

```text
1. stores/readers CURRENT de ada-generic-application
2. contrato Tool CURRENT realmente consumible por ADA Generic
3. contratos exactos de Latest y Timeseries readers
4. ownership del polling/scheduling existente
5. coherencia requerida al escribir/actualizar stores UI
6. comportamiento ante Latest nuevo con Timeseries todavía anterior
7. comportamiento ante ausencia temporal de una de las dos superficies
```

Luego introducir sólo el gap mínimo.

## No asumir

```text
Collector == nuevo servicio
Collector == Producer renombrado
1 Subcomponent == 1 Store
un único intervalo para ambas superficies
atomicidad cross-document no demostrada
nuevo schema intermedio
```
