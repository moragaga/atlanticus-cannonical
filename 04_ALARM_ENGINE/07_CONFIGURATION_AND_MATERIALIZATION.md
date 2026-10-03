# Alarm Engine — Configuration and Materialization

Estado: **CURRENT implementation + target Runtime/Modeler/Delivery split DESIGN FROZEN; implementation PLANNED**.

Implementation checkpoint relevante de Alarm:

```text
atlanticus@346e7ac7ba7c21eede8b524613a6adee7e839e55
```

Repository HEAD inspeccionado:

```text
atlanticus@09e9acf6edf6f84a66a4a0a041ad9a8f645daf79
```

Los commits entre ambos no modifican rutas Alarm.

## 1. Published configuration CURRENT

Shared configuration vive en `ada-contracts-alarms`.

```text
AlarmConfigurationSnapshot
    configuration: AlarmConfiguration
    tool_dependencies: ToolDependencyManifest
```

El snapshot publicado preserva la Tool Catalog revision confirmada.

## 2. Command Center semantic ownership — FROZEN

Antes de publicación, Command Center posee:

```text
authoring
business validation
Tool reference validation/resolution
routing validation
visual target validation
publication
```

Una publicación debe llegar al Engine ya semanticamente válida/materializable.

Materialization target no debe rediscover Tools ni repetir semantic resolution.

## 3. Materialization CURRENT implementado

Actualmente produce:

```text
RuntimeAlarmConfiguration
DeliveryAlarmConfiguration
```

`DeliveryAlarmConfiguration` mezcla información de modelado y entrega.

Campos verificados relevantes:

```text
ResolvedDeliveryAlarm
    identity
    is_active
    visibility_mode
    display_name
    title
    cause_template
    kind
    criticality
    business_category
    operational_areas
    color
    deactivation policies/messages
    visual_targets

ResolvedVisualTarget
    tool_key
    tool_kind
    component_keys
    subcomponents
    process_projection_mode
```

`tool_kind` usa actualmente `ada.contracts.tools.ToolConfigurationKind`.

`process_projection_mode` usa:

```text
GENERIC
DISTRIBUTED
```

## 4. Materialization target — REFINED / DESIGN FROZEN

SUPERSEDED como target:

```text
published AlarmConfigurationSnapshot
        ↓
RuntimeAlarmConfiguration
DeliveryAlarmConfiguration
```

Target:

```text
published AlarmConfigurationSnapshot
        ↓
deterministic materialization
        ├── RuntimeConfiguration
        ├── ModelerConfiguration
        └── DeliveryConfiguration
```

Los tres artefactos deben formar parte del mismo result/materialization y compartir el exact artifact pin.

## 5. RuntimeConfiguration target

Contiene exclusivamente lo necesario para evaluación/ejecución operacional:

```text
defined alarm identities
planned alarms
evaluator references
runtime parameters
priority/lifecycle execution inputs
```

No contiene carousel, queue-in-queue, component/subcomponent projection logic ni destino físico de publicación.

## 6. ModelerConfiguration target

Debe recibir la información pre-resuelta necesaria para producir el estado lógico consumible por una proyección.

Candidatos ya observados en el contrato actual:

```text
alarm identity
visibility
display semantic metadata
messages/enrichment data needed downstream
logical target key
component keys
subcomponent addresses
process projection mode
```

Además deberá expresar la estrategia/configuración necesaria para:

```text
CAROUSEL
QUEUE_IN_QUEUE
rotation windows
logical capacity/partitioning
```

### Exact schema — OPEN

No inventar todavía:

```text
model type enum
queue_in_queue variant field
fairness policy fields
rotation default
```

Deben derivarse del contrato/Tool configuration real o definirse explícitamente en el siguiente incremento.

## 7. DeliveryConfiguration target

Debe quedar reducida a información de entrega/transporte ya resuelta.

Responsabilidad target:

```text
destination identity
physical connection/reference
container/topic/store target
transport/publication options
```

### Exact fields — OPEN

No congelar `connection_ref`, `container_name` u otros detalles como contrato público hasta inventariar cómo se provisionan las proyecciones actuales.

Principio congelado:

```text
Delivery no decide modelado.
Delivery publica documentos ya modelados.
```

## 8. Tool dependency cleanup — FROZEN direction

El Engine target no debe depender de `ada-contracts-tools` sólo para transportar algunos valores externos.

No crear:

```text
EngineToolKind mirrors ToolConfigurationKind
EngineToolStructure mirrors ToolStructure
adapters sólo para leer enum/attributes
```

Regla:

```text
transport-only external value
    -> primitive/documented value

Alarm semantic decision
    -> Alarm-owned type
```

`ToolDependencyManifest` puede seguir siendo provenance/audit del snapshot publicado, pero no obliga a que el Modeler/Runtime consuma tipos Tool completos.

## 9. Residual Command Center domain

`next_routing_tool_kind()` pertenece conceptualmente a validación/routing pre-publicación en Command Center.

`ALARM_CONFIGURATION_SOURCE_KEY` es identidad de publicación compartida/primitiva; no justifica un módulo completo como dependencia del Engine.

La rehome/eliminación física sigue OPEN hasta inventario final.

## 10. Exact pin/adoption invariants — FROZEN

```text
READY != EFFECTIVE
source_key + result_id + manifest_sha256 + resolution_key
Runtime, Modeler y Delivery usan el mismo exact artifact
no fallback to latest READY
```

## 11. Current implementation debt

Materialization todavía contiene semantic checks y dependencias hacia Web/projection packages del modelo anterior.

Mover ese código tal cual sería mover deuda de frontera.

La extracción debe limpiar la frontera, no conservarla mediante adapters.
