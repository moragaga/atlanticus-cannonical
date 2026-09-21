# ADA Generic — Scope

Estado: **CURRENT / STAGE 1 CLOSED**

## Propósito

ADA Generic compone la base reutilizable necesaria para ejecutar una ADA sobre contratos
configurados y entregar estado operacional consumible.

No debe convertirse en una megaaplicación ni absorber comportamiento visual específico de cada
Tool.

## Frontera de ownership

Atlanticus aporta infraestructura y capacidades genéricas.

ADA puede poseer capabilities propias bajo `scopes/ada` y consumir esos contratos genéricos
directamente.

Integrar una capability no transfiere su ownership al core.

El núcleo genérico de Atlanticus nunca depende de ADA.

## Alcance CURRENT de ADA Generic

Stage 1 resuelve:

```text
environment / .env
→ provider settings
→ Tool Projection durable
→ Tool resolution
→ ToolStructure
→ KPI Collector cuando corresponde
→ Latest / Timeseries
→ process cache
→ browser dcc.Store por ToolComponent
```

La aplicación puede continuar existiendo aunque una capability operacional no esté configurada o
esté temporalmente indisponible, de acuerdo con sus estados de resolución.

## Frontera de entrega

La frontera genérica termina en la entrega del estado operacional al consumidor Web.

```text
ADA Generic
→ dcc.Store por ToolComponent
→ END GENERIC DATA DELIVERY
```

Desde ahí:

```text
developer / concrete Tool application
→ construye la visualización específica
```

Esto permite que una Tool como Operaciones Integradas tenga necesidades visuales especiales sin
convertirlas en arquitectura obligatoria para Mina, Process u otras Tools.

## Operational Render

`OperationalRenderBinding` conserva únicamente estructura:

```text
ToolStructure
ToolComponent
```

No contiene estado KPI.

No contiene `ComponentStoreSnapshot`.

No representa una segunda ruta de datos.

## Regla canónica

```text
CONFIGURATION
determines structure

RUNTIME DATA
determines state

GENERIC APPLICATION
delivers stable contracts and data boundaries

CONCRETE TOOL / DEVELOPER
owns specific visualization
```

No crear adapters, shims, aliases ni doble contrato para unir estas fronteras.

## Estado

```text
ADA-GENERIC-STAGE-1
CLOSED / VERIFIED / CURRENT
```

Nuevos trabajos en ADA Generic requieren un finding real descubierto por consumidores concretos.
