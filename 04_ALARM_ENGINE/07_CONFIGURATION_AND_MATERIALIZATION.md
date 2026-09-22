# Alarm Engine — Configuration and Materialization

Estado: **B.2 CONTRACTS + PURE RESOLVER CURRENT / MATERIALIZATION PROCESS PLANNED NEXT / ADOPTION + LIVE DELIVERY NOT IMPLEMENTED**

## Authority checkpoint

Implementación auditada:

```text
moragaga/atlanticus:main
9398786ae9af7c00de1bcca9d7a311fe9ef2155f
```

Canonical inspeccionado antes de este reemplazo:

```text
moragaga/atlanticus-cannonical:main
7fea2819aa4c22f9d7494cbe79ab8740ee3f4366
```

Decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
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

B.2 no elimina silenciosamente una Rule rota para construir un artifact parcial.

## Owner CURRENT

```text
scopes/ada-command-center/backend/alarms/materialization
```

Distribución:

```text
ada-command-center-alarms-materialization==1.0.0
```

Namespace:

```python
ada_command_center.alarms.materialization
```

Dependencias declaradas CURRENT:
- Alarm Core;
- Alarm Domain;
- `ada-web-tools`.

El resolver no agregó dependencia directa al package operacional `ada-command-center-tools-catalog`.
Consume estructuralmente el catálogo confirmado por:
- `revision`;
- `get(tool_key)`;
- entries con `kind` y `structure`.

Esto permite recibir el `ToolCatalogSnapshot` real sin acoplar Materialization a stores,
consolidator o dependencias de infraestructura del package de catálogo.

## AlarmResolutionKey CURRENT

Owner físico:

```text
ada_command_center.alarms.core.AlarmResolutionKey
```

Shape:

```text
alarm_configuration_revision
confirmed_tool_catalog_revision
```

El resolver construye la key desde:
- `alarm_configuration_revision` explícita;
- `confirmed_tool_catalog.revision`.

No se pasa una revisión Tool separada.

No contiene evaluator revision.

## Resolution contracts CURRENT

```text
AlarmResolutionStatus
    READY
    BLOCKED

AlarmResolutionFindingSeverity
    BLOCKING
    WARNING

AlarmResolutionFinding
    code
    severity
    message
    alarm_identity?
    field_path?
    reference_key?

AlarmConfigurationResolution
    resolution_key
    status
    findings
    runtime_configuration?
    delivery_configuration?
```

Atomicidad:

```text
READY
<=> no BLOCKING
<=> Runtime existe
<=> Delivery existe
<=> Runtime.key == Delivery.key == resolution key

BLOCKED
<=> al menos un BLOCKING
<=> Runtime is None
<=> Delivery is None
```

No existe readiness parcial.

## Pure B.2 resolver CURRENT

Punto de entrada:

```python
resolve_alarm_configuration(
    configuration,
    alarm_configuration_revision,
    confirmed_tool_catalog,
    tool_qualification,
    evaluator_qualification,
) -> AlarmConfigurationResolution
```

Frontera:

```text
AlarmConfiguration
+ alarm_configuration_revision
+ Confirmed Tool Catalog
+ ToolReconciliationQualification
+ EvaluatorQualificationCatalog
        |
        v
pure deterministic resolver
        |
        v
AlarmConfigurationResolution
```

El resolver:
- no hace I/O;
- no lee Cosmos, Blob, SharePoint ni Runtime stores;
- no adquiere qualification;
- no persiste artifacts;
- no decide EFFECTIVE;
- no ejecuta scheduler/orchestration;
- no reimplementa priority, Management suppression, deactivation cascade ni Special Condition
  Runtime behavior.

Acquisition pertenece al futuro Materialization Process.

## Qualification externa CURRENT

Evaluator:
- toda Rule definida debe estar qualified por `(family_key, evaluator_key)`;
- aplica incluso a Rules disabled.

Tools:
- origin;
- cada escalation target, incluso step disabled;
- cada visual target;
- deben existir en el catálogo confirmado exacto y estar GREEN.

Finding codes CURRENT:

```text
evaluator_not_qualified
tool_reference_not_found
tool_reference_not_green
routing_invalid_for_criticality
visual_target_invalid
```

Todos los findings usados hoy son `BLOCKING`.
`WARNING` existe en el contrato, pero el resolver CURRENT no inventa warnings.

El recorrido y orden de findings son determinísticos.

## Routing CURRENT

Los steps se consideran por `step_order`.

### C1

Enabled step:

```text
wait_minutes_from_previous_step in {None, 0}
-> RoutingDestination(delay_seconds=None)
```

Un wait positivo bloquea el candidato.

### C2

Cada enabled step requiere:

```text
wait_minutes_from_previous_step > 0
```

La definición authored expresa espera relativa respecto del step ejecutable previo.

Runtime requiere offsets absolutos desde el inicio de la occurrence.

Por tanto:

```text
waits 10, 5, 7 minutos
-> delay_seconds 600, 900, 1320
```

Los steps disabled:
- no se materializan como destinos;
- no contribuyen al acumulado temporal;
- su `target_tool_key` sigue siendo una referencia definida y debe existir + estar GREEN.

### C3

No admite enabled escalation steps.

Runtime:

```text
AlarmRouting(origin_tool_key=..., destinations=())
```

## Runtime Configuration CURRENT

```text
RuntimeAlarmConfiguration
    resolution_key
    defined_alarm_identities
    planned_alarms
    parameters_by_alarm
```

Representación:

```text
active executable
-> defined + PlannedAlarm + parameters

disabled
-> defined + no PlannedAlarm + no parameters

removed
-> absent
```

`PlannedAlarm` materializado recibe:
- identity/kind/criticality/priority;
- evaluator key;
- revisions alineadas con `AlarmResolutionKey`;
- routing;
- `DeactivationPolicy` sólo si default deactivation está enabled;
- reappearance timer;
- reappearance Special Conditions.

Conversión:

```text
ReappearanceDefinition.after_minutes is None
-> reappearance_after_seconds = None

ReappearanceDefinition.after_minutes = M
-> reappearance_after_seconds = M * 60
```

## Delivery Configuration CURRENT

Delivery conserva toda Rule definida, incluidas:
- disabled;
- TRACE_ONLY.

Message selection:
- referencias inválidas ya son rechazadas por `AlarmConfiguration`;
- inactive Message permanece definición válida;
- inactive Message no se materializa como opción para nueva gestión;
- sin override: usa default deactivation de la Rule;
- con override: reemplazo completo, no merge campo a campo.

Visual targets:
- Tool debe existir y estar GREEN;
- Process requiere `process_projection_mode`;
- Integrated Operations no admite `process_projection_mode`;
- Strategic queda BLOCKED mientras Alarm projection siga indefinida;
- component/subcomponent deben existir;
- Delivery conserva referencias estables;
- no copia `ToolStructure`.

## Responsabilidad del Domain que B.2 no duplica

`AlarmConfiguration` sigue siendo owner de:
- identity uniqueness;
- `rule_name` uniqueness por family;
- prioridad/cross-rule authoring invariants;
- Message existence/scope;
- Special Condition existence/family/priority_group;
- invariantes locales de definitions.

El resolver no vuelve a implementar esas validaciones.

## Qualification observada

Ejecución compartida con CPython 3.14.2:

```text
uv run pytest
31 passed in 0.07s

uv run ruff check .
All checks passed!
```

El último output de:

```text
uv run ruff format --check .
```

mostrado antes del commit todavía indicaba dos `resolver.py` por reformatear.

El checkpoint CURRENT `9398786...` contiene versiones posteriores del incremento, pero no se preservó
en este cierre una nueva salida explícita del formatter.

Estado de evidencia:

```text
functional tests     VERIFIED / GREEN
lint                 VERIFIED / GREEN
final formatter gate UNVERIFIED
implementation main  VERIFIED / CURRENT
```

## Materialization Process — NEXT / NOT IMPLEMENTED

Target físico acordado:

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

Responsabilidad futura:
- acquisition de inputs;
- revision comparison cuando corresponda;
- creación/adquisición de ToolReconciliationQualification;
- creación/adquisición de EvaluatorQualificationCatalog;
- invocación del resolver puro;
- persistencia/publicación de artifacts y findings;
- diagnostics/telemetry/retry/exit behavior.

No asumir automatización total: el proceso puede incluir operación humana controlada si el contrato lo
requiere; esa decisión operacional todavía no está cerrada.

## Runtime gaps separados

OPEN:
- provenance histórica de `PlannedAlarm`/occurrence -> `AlarmResolutionKey`;
- reconciliation de reappearance timer/SC sobre hot state durante Runtime Adoption;
- cleanup de `PlannedAlarm.deactivation_policy`;
- Runtime Adoption/Effective Head.

El resolver no debe convertir estos gaps en adapters o contratos paralelos.

## Runtime Adoption / EFFECTIVE

Sigue NOT IMPLEMENTED.

```text
B.2 READY != EFFECTIVE
```

## Live Delivery

Contrato de diseño permanece acordado y no fue implementado.

Exact-key alignment:

```text
EngineResolvedCurrentState.resolution_key
==
DeliveryAlarmConfiguration.resolution_key
==
AlarmEffectiveConfigurationHead.resolution_key
```

## OPEN después de este checkpoint

- evidence final de formatter del resolver;
- productor/adquisición de current Tool reconciliation GREEN;
- productor/adaptación de evaluator qualification;
- `backend/processes/alarms-materialization`;
- Runtime/Delivery/findings artifact stores;
- codecs/schema versions/retention;
- cause/evidence schema;
- Runtime provenance cleanup;
- reappearance reconciliation durante Runtime Adoption;
- deactivation Core ownership cleanup;
- Runtime Adoption + Effective Head;
- Live Delivery implementation;
- Management Capture;
- routing tier/kind constraints adicionales;
- Rule area vs Tool scope qualification;
- cadence/event trigger/deployment topology;
- Python baseline conflict.

## No reabrir por conveniencia

B.2 resolver CURRENT no debe:
- adquirir datos;
- leer stores;
- persistir;
- reimplementar priority;
- reimplementar Management suppression;
- reimplementar deactivation cascade;
- reimplementar Special Condition Runtime reappearance;
- introducir UI semantics en Core;
- introducir aliases/adapters legacy.
