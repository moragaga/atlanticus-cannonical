# Alarm Engine — Command Center Analytics Boundary

Estado: **CURRENT — LIVE BOUNDARY IMPLEMENTED HISTORICALLY; MANAGEMENT/HISTORY SEPARATE**

## Live path CURRENT contract

```text
Runtime operational truth
    ↓
Modeler
    ↓ per-Tool current logical projection
Delivery
    ↓
Cosmos alarm-live-projection
    ↓
Web consumer
```

Live Projection contiene estado current para operación visual.

No es History/Analytics.

El Runtime ejecutable permanece bloqueado por su integración de Operational Data; este diagrama expresa la frontera contractual vigente y el pipeline históricamente cualificado.

## Web alarm-surface foundation — NEXT

La primera card de alarmas y la integración de `alarm-management` / `alarm-status` deben consumir contratos/projections Web apropiados.

No deben introducir:

```text
direct WAL reads
direct Runtime CURRENT reads as final UI contract
analytics reconstruction inside callbacks
engine lifecycle decisions inside Dash
```

## Analytics path — PLANNED / SEPARATE

```text
Runtime durable FACTS
    ↓
History / Analytics materialization
    ↓
Analytics read model
    ↓
Command Center analytics surfaces
```

Analytics debe derivarse de hechos durables y contratos explícitos, no del current live head como sustituto de historia.

## Management path — PLANNED / SEPARATE

Management/deactivation puede requerir captura y proyecciones propias.

No mezclarlo con `operator_view` del live baseline.

`alarm-management` como superficie Web no convierte automáticamente el live projection en Management Projection.

## Frozen separation

```text
Live Projection
Management Projection
History / Analytics
```

son superficies distintas.

Web no lee WAL directo.

Web no lee Runtime CURRENT directo como contrato final de visualización.

Analytics no modifica Engine state.

Delivery no calcula analytics.

Modeler live no reconstruye History.

## Useful historical facts for Analytics

Cuando se implemente, fuentes potenciales incluyen:

```text
Occurrence/Episode
Journey
Evidence
management/deactivation
routing/assignments
priority transitions
configuration/tool revisions
projection transitions if separately persisted
```

No inferir que el live snapshot conserva todas esas transiciones.

## Engine migration constraint

La futura migración a `ada-alarm-engine` debe preservar esta separación.

Eliminar campos o simplificar contratos operacionales no autoriza a colapsar Live, Management y History en un único modelo.
