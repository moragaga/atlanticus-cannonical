# ADA Command Center — Domain Ownership and Alarm Configuration Migration

Estado: **CURRENT / DOMAIN MIGRATION CLOSED / MATERIALIZATION OWNER IMPLEMENTED**

## 1. Authority checkpoint

Implementación CURRENT:

```text
moragaga/atlanticus:main
bc3fffd72afb712d5b5ab84522c379abf2a19642
```

Canonical base:

```text
moragaga/atlanticus-cannonical:main
2d8cbc33b7776e057e4f7d82def318d5eaf8f336
```

Decisions:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## 2. Authored Domain CLOSED

```text
scopes/ada-command-center/domain/alarms
ada-command-center-alarms-domain==1.0.0
ada_command_center.domain.alarms
```

Una única autoridad contractual para Alarm Configuration.

No depende de Web, Runtime, Persistence ni infraestructura.

## 3. Root replacement CLOSED

Removidas como authorities:

```text
backend/alarms/core/definition.py
web/alarms/configuration/models.py
```

Sin aliases/re-exports legacy.

## 4. Backend Core ownership CURRENT

Core conserva Runtime/lifecycle contracts.

También es owner físico de:

```text
AlarmResolutionKey
```

porque es identidad operacional compartida y deberá ser usable por Core/Adoption sin crear
dependencia Core -> Materialization.

Shape:

```text
alarm_configuration_revision
confirmed_tool_catalog_revision
```

## 5. Materialization ownership CURRENT

Implementado:

```text
scopes/ada-command-center/backend/alarms/materialization
ada-command-center-alarms-materialization==1.0.0
ada_command_center.alarms.materialization
```

Responsabilidad:
- pure resolution DTOs;
- Runtime Configuration contract;
- Delivery Configuration contract;
- findings/status;
- qualification input contracts.

No es owner de:
- authored AlarmDefinition/Configuration;
- Engine lifecycle;
- Tool discovery;
- evaluator runtime code;
- I/O;
- stores;
- process orchestration.

## 6. Dependency direction CURRENT

```text
domain.alarms
      ^
      |
alarms.core
      ^
      |
alarms.materialization
```

Materialization también consume `ada-web-tools` únicamente para `ToolConfigurationKind` en resolved
visual targets.

Domain no depende hacia arriba.

Core no depende de Materialization.

## 7. Separación congelada

```text
AUTHORED DOMAIN
AlarmConfiguration
AlarmDefinition
MessageDefinition

RUNTIME CORE
PlannedAlarm
AlarmResolutionKey
Engine state/lifecycle

RUNTIME RESOLVED
RuntimeAlarmConfiguration

DELIVERY RESOLVED
DeliveryAlarmConfiguration
ResolvedDeliveryAlarm
ResolvedDeliveryMessage
ResolvedDeactivationPolicy
ResolvedVisualTarget
```

No mover artifacts resueltos al authored Domain.

## 8. Runtime visibility cleanup CLOSED

Removido de Core:

```text
delivery_enabled
PriorityDisposition.SHADOW
```

No reemplazar por otro visibility flag.

## 9. Qualification inputs CURRENT

```text
ToolReconciliationQualification
EvaluatorQualificationKey
EvaluatorQualificationCatalog
```

Son contratos de consumo B.2, no owners del sistema que produce reconciliation/evaluator state.

## 10. Process ownership PLANNED

Future orchestration:

```text
scopes/ada-command-center/backend/processes/alarms-materialization
```

Todavía ausente.

Responsabilidad futura:
- acquire inputs;
- compare revisions;
- invoke pure resolver;
- persist artifacts/findings;
- diagnostics.

No mover esa responsabilidad al contract package.

## 11. SUPERSEDED

Siguen reemplazadas propuestas de authored contract bajo:

```text
backend/alarms/configuration
alarms/configuration/core
```

Authority authored permanece:

```text
domain/alarms
```

También queda refinada la propuesta inicial de ubicar `AlarmResolutionKey` en Materialization:
CURRENT owner es Core.

Esto no crea legacy; es la única autoridad implementada del key.

## 12. OPEN separado

- pure B.2 resolver;
- producer/adapters concretos de qualification inputs;
- process orchestration;
- artifact stores;
- Runtime provenance cleanup;
- Runtime Adoption;
- Live Delivery;
- Management Capture.

## 13. Python conflict

```text
Project 3.14.7
Command Center requires-python ==3.14.2
```

OPEN.

## 14. Foco único siguiente

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
```

Primero diseño contra contracts CURRENT; luego implementación incremental.

No mezclar con process, I/O, Adoption, Delivery ni provenance migration.
