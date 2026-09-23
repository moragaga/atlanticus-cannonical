# Alarm Engine — Configuration and Materialization

Estado: **CURRENT / PURE RESOLVER IMPLEMENTED / EXACT DEPENDENCY SNAPSHOT IMPLEMENTED / MATERIALIZATION PROCESS PLANNED**

## Authority

```text
Implementation CURRENT
880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6

Alarm/Tool snapshot close
d2a5e14822d3711e64668b8e70cfa15d7ddae2f0
```

## Invariantes centrales

```text
LATEST SAVED = LATEST VALID_AT_SAVE
VALID_AT_SAVE != READY != EFFECTIVE

INVALID != REMOVED
DISABLED != INVALID
DISABLED != REMOVED
TRACE_ONLY != REMOVED
READY != EFFECTIVE
```

`VALID_AT_SAVE` no equivale a qualification B.2 completa.

## Pure B.2 resolver CURRENT

Owner:

```text
scopes/ada-command-center/backend/alarms/materialization
```

Entrada conceptual:

```text
AlarmConfiguration
+ alarm_configuration_revision
+ exact confirmed Tool evidence
+ ToolReconciliationQualification
+ EvaluatorQualificationCatalog
        |
        v
resolve_alarm_configuration(...)
        |
        v
AlarmConfigurationResolution
```

El resolver sigue siendo puro:
- sin I/O;
- sin stores;
- sin scheduler;
- sin acquisition;
- sin persistence;
- sin Runtime Adoption.

## Exact Tool evidence CURRENT

Cada Alarm source release publicada persiste:

```text
AlarmConfigurationSnapshot
    configuration
    tool_dependencies: ToolDependencyManifest
```

`ToolDependencyManifest` expone:

```text
revision
get(tool_key)
```

Cada entry expone:

```text
kind
structure
```

Por tanto satisface la frontera estructural de catálogo que B.2 consume.

También conserva:
- `display_name`;
- `source_release_id`;
- Component/Subcomponent display names dentro de `ToolStructure`.

## Referencias incluidas

```text
cada Rule escalation.origin_tool_key
cada escalation step target_tool_key
cada visual target tool_key
```

No se filtra por `rule.is_active` ni `step.is_enabled`.

El manifest persiste sólo el subset referenciado, conservando la revisión del catálogo confirmado
completo `Cn`.

## Tool correlation

```text
R1/C1
```

permanece válido aunque aparezca `C2`.

No hacer:

```text
R1 + latest C2
```

sin nueva publicación Alarm.

## Qualification B.2

Evaluator:
- toda Rule definida debe estar qualified por `(family_key, evaluator_key)`.

Tools:
- toda Tool reference definida debe existir en la evidencia exacta;
- `ToolReconciliationQualification` sigue siendo input explícito;
- productor concreto todavía OPEN.

Finding codes CURRENT:

```text
evaluator_not_qualified
tool_reference_not_found
tool_reference_not_green
routing_invalid_for_criticality
visual_target_invalid
```

## Atomicidad

```text
READY
=> Runtime artifact
=> Delivery artifact
=> same resolution_key

BLOCKED
=> blocking finding
=> Runtime None
=> Delivery None
```

## C1/C2/C3 CURRENT

C1:
- enabled step inmediato;
- wait positivo bloquea.

C2:
- enabled waits relativos;
- Runtime recibe offsets absolutos acumulados;
- disabled steps no se ejecutan ni contribuyen al acumulado;
- su Tool reference sí se califica.

C3:
- no admite enabled escalation steps.

## Materialization Process — PLANNED / AFTER OPERATIONAL PROJECTION

Target previsto:

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

No implementado.

Responsabilidad futura:
- adquirir Alarm Configuration Projection operacional;
- adquirir Tool/Evaluator qualification;
- comparar revisions;
- invocar resolver;
- persistir Runtime/Delivery/findings;
- diagnostics/retry/telemetry.

No volver a abrir Tool Catalog histórico dentro del process.

## Siguiente prerequisite

```text
Alarm Configuration operational Projection to Cosmos
```
