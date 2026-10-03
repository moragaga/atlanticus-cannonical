# Alarm Engine — Configuration and Materialization

Estado: **CURRENT — TWO-ARTIFACT READY CONTRACT; MODELER BASELINE CONSUMES EXISTING DELIVERY CONFIGURATION**

Checkpoint:

```text
atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856
```

## Published configuration CURRENT

```text
AlarmConfigurationSnapshot
    configuration: AlarmConfiguration
    tool_dependencies: ToolDependencyManifest
```

Command Center continúa siendo responsable antes de publicar de authoring, semantic validation, Tool/reference resolution y visual/routing validation.

## Materialization CURRENT

Produce:

```text
RuntimeAlarmConfiguration
DeliveryAlarmConfiguration
```

READY exige ambos, el mismo `resolution_key`, manifest íntegro y exact artifact identity.

## Modeler baseline — refinement

La decisión previa que declaraba obligatorio producir:

```text
RuntimeConfiguration
ModelerConfiguration
DeliveryConfiguration
```

antes de implementar Modeler queda **SUPERSEDED como prerequisito**.

La implementación CURRENT demuestra un baseline coherente usando:

```text
RuntimeAlarmConfiguration
+
DeliveryAlarmConfiguration
```

El Modeler consume de `DeliveryAlarmConfiguration` metadata estática y `visual_targets` ya resueltos; no rediscover Tools.

## Delivery configuration CURRENT

Incluye información downstream hoy utilizada tanto por Modeler como por Delivery boundary, por ejemplo:

```text
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
visual_targets
```

`ResolvedVisualTarget` conserva Tool/component/subcomponent/process projection metadata.

## Future split — OPEN, not mandatory

Crear `ModelerConfiguration` separado sólo si el scheduler completo introduce configuración con responsabilidad independiente real, por ejemplo:

```text
rotation policy
logical capacity
QIQ policy
scheduler strategy
```

No crear el split sólo por simetría.

## Exact pin invariants — FROZEN

```text
READY != EFFECTIVE
source_key + result_id + manifest_sha256 + resolution_key
Runtime, Modeler y Delivery usan el mismo exact artifact
no fallback to latest READY
```

## External Tool dependency direction

Engine no debe rediscover Tool Catalog durante ejecución. Provenance/tool snapshot publicado puede existir, pero las decisiones operacionales deben usar el artifact ya materializado.
