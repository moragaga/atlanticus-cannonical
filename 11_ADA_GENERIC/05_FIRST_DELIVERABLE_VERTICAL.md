# ADA Generic — First Deliverable Vertical

Estado: **CURRENT / GENERIC STAGE 1 CLOSED / TOOL GOLDEN PATH OPEN**

## Separación de hitos

La base genérica y la primera vertical completa de una Tool no son el mismo entregable.

### ADA Generic Stage 1

CLOSED:

```text
Tool Projection durable
→ resilient Tool resolution
→ ToolStructure
→ KPI Collector
→ Latest / Timeseries
→ process cache
→ browser dcc.Store / ToolComponent
→ developer handoff
```

La base genérica entrega datos y contratos.

No posee la visualización específica de una Tool.

### Tool Golden Path

Permanece OPEN.

Debe demostrar una Tool concreta recorriendo, según corresponda:

```text
Configuration
→ Source / Release
→ Projection / materialization
→ Operational Data
→ KPI
→ Alarm
→ concrete visualization
→ Manager / operational workflows
→ E2E
```

## Operaciones Integradas

El orden funcional vigente permanece:

```text
1. Operaciones Integradas
2. Mina
```

Operaciones Integradas es la primera Tool destinada a revelar gaps reales de consumo, layout,
alarm integration y distribución.

Sus casos visuales especiales no deben generalizarse automáticamente dentro de ADA Generic.

## Criterio de cierre de ADA Generic Stage 1

Cumplido:

- runtime Tool read desde Projection durable;
- estados degradados no eliminan Web base;
- Collector wiring sobre Tool `READY`;
- Latest y Timeseries con cadencias independientes;
- un browser store por ToolComponent;
- Subcomponent sin store propio;
- separación estructural de Operational Render;
- frontera explícita de entrega al desarrollador.

## Lo que no pertenece al cierre genérico

No es requisito de Stage 1:

```text
body universal
cards KPI obligatorias
layout de Operaciones Integradas
visualización de Mina
alarm visualization concreta
```

Esas decisiones pertenecen a consumidores concretos.

## Siguiente frontera del Project

Antes de continuar el Golden Path visual de una Tool, el siguiente foco solicitado es:

```text
ADA-COMMAND-CENTER-ALARM-CONFIGURATION
PLANNED / NEXT
```
