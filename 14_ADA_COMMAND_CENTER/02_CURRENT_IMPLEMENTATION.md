# ADA Command Center — Current Implementation

Estado: **VERIFIED / UPDATED 2026-09-22**

Corte auditado:

```text
moragaga/atlanticus@bc3fffd72afb712d5b5ab84522c379abf2a19642
```

## Físicamente en main

```text
scopes/ada-command-center/
├── domain/
│   └── alarms/
├── backend/
│   ├── alarms/
│   │   ├── core/
│   │   ├── materialization/
│   │   └── persistence/
│   ├── processes/
│   │   └── alarms-runtime/
│   └── tools/
│       └── catalog/
└── web/
    ├── alarms/
    │   └── configuration/
    └── application/
        └── ada-command-center-configuration-manager/
```

Todavía no existe:

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

## Clasificación CURRENT

```text
Alarm Domain shared package                         IMPLEMENTED / VERIFIED / CURRENT
Backend Alarm Engine                                IMPLEMENTED / CURRENT
Alarm Core runtime visibility cleanup               IMPLEMENTED / VERIFIED / CLOSED
Alarm Configuration Source/Release                  IMPLEMENTED / CURRENT
Alarm Configuration base Projection                IMPLEMENTED / CURRENT
Command Center Tool Catalog V1                     IMPLEMENTED / CURRENT
Alarm Tool Reference read model V1                 IMPLEMENTED / CURRENT
B.2 Materialization contract package               IMPLEMENTED / VERIFIED / CURRENT
B.2 Qualification input contracts                  IMPLEMENTED / VERIFIED / CURRENT
Pure B.2 resolver                                   NOT YET IMPLEMENTED / NEXT
B.2 Materialization process                        NOT YET IMPLEMENTED
Runtime Effective Head                             NOT YET IMPLEMENTED
Alarm Live Delivery                                NOT YET IMPLEMENTED
Management Projection                              NOT YET IMPLEMENTED
```

## Alarm Domain CURRENT

```text
scopes/ada-command-center/domain/alarms
ada-command-center-alarms-domain==1.0.0
ada_command_center.domain.alarms
```

Authority:
- `AlarmIdentity`;
- `AlarmKind`;
- `Criticality`;
- authoring definitions;
- `MessageDefinition`;
- `AlarmDefinition`;
- `AlarmConfiguration`;
- pure aggregate validation.

No productive dependencies.

## Alarm Core CURRENT

`AlarmResolutionKey` es público en Core:

```text
alarm_configuration_revision
confirmed_tool_catalog_revision
```

Runtime visibility root removal CLOSED:

```text
delivery_enabled  absent
SHADOW            absent
```

Priority CURRENT:

```text
PREDOMINANT
ECLIPSED
CASCADE_SUPPRESSED
DEACTIVATED
```

## B.2 Materialization CURRENT

Package:

```text
scopes/ada-command-center/backend/alarms/materialization
```

Distribución:

```text
ada-command-center-alarms-materialization==1.0.0
```

Módulos productivos:

```text
__init__.py
delivery.py
qualification.py
resolution.py
runtime.py
py.typed
```

Mirror pedagógico equivalente bajo `commented/`.

Tests:

```text
support.py
test_delivery.py
test_qualification.py
test_resolution.py
test_runtime.py
```

## Resolution CURRENT

```text
READY
BLOCKED
```

`READY` requiere ambos artifacts y misma key.

`BLOCKED` requiere al menos un BLOCKING y prohíbe artifacts parciales.

## Runtime artifact CURRENT

```text
RuntimeAlarmConfiguration
    resolution_key
    defined_alarm_identities
    planned_alarms
    parameters_by_alarm
```

Disabled sigue definida sin plan.

Removed está ausente.

## Delivery artifact CURRENT

Incluye configuration ya resuelta para Delivery:
- visibility;
- display/title/cause template;
- kind/criticality/category/areas/color;
- deactivation policy;
- Messages;
- visual targets.

No contiene evaluator, parameters, routing, lifecycle ni ToolStructure.

## Qualification inputs CURRENT

```text
ToolReconciliationQualification(green_tool_keys)
EvaluatorQualificationKey(family_key, evaluator_key)
EvaluatorQualificationCatalog(qualified_keys)
```

No implementan discovery/reconciliation ni evaluator runtime registry.

## Qualification observada durante integración

Antes del primer B.2 commit:

```text
Materialization
14 passed
ruff check: GREEN
```

Antes del Qualification Inputs commit:

```text
Materialization
19 passed
ruff check: GREEN
```

En esa corrida `ruff format --check` solicitó formato sobre los dos nuevos `qualification.py`;
posteriormente el commit `bc3fffd72afb712d5b5ab84522c379abf2a19642` contiene esos archivos formateados.

No se capturó en esta conversación una rerun completa de `pytest/ruff` sobre los bytes exactos
posteriores al último format y al commit.

Por tanto:

```text
implementation bytes at bc3fffd72afb712d5b5ab84522c379abf2a19642   VERIFIED
pre-final-format 19-test run              VERIFIED
exact post-commit full gate               UNVERIFIED
```

Core había observado:

```text
177 passed
ruff check: GREEN
ruff format --check: GREEN
```

antes del incremento Qualification Inputs; ese incremento no modificó Core.

## Root replacements / no legacy

No existen authorities authored duplicadas en Core/Web.

No se introdujeron aliases, shims ni adapters para B.2.

## Conflicto técnico visible

```text
Project baseline       Python 3.14.7
Command Center pins    requires-python ==3.14.2
```

OPEN.

## Siguiente incremento

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
```
