# Alarm Engine — Command Center Analytics Boundary

Estado: **CURRENT — LIVE BOUNDARY IMPLEMENTED; MANAGEMENT/HISTORY SEPARATE**

## Live path CURRENT

```text
Runtime operational truth
    ↓ CURRENT
Modeler
    ↓ per-Tool current logical projection
Delivery
    ↓
Cosmos alarm-live-projection
    ↓
Command Center Web [consumer NEXT]
```

Live Projection contiene estado current para operación visual. No es History/Analytics.

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

Management/deactivation puede requerir captura y proyecciones propias. No mezclarlo con `operator_view` del live baseline.

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
