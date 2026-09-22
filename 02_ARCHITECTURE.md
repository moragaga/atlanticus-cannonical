# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA.

## ADA Command Center layers

CURRENT:

```text
scopes/ada-command-center/
├── domain/
├── backend/
└── web/
```

`domain/` contiene contratos funcionales puros con consumidores independientes en Web y Backend.

Authority authored de Alarm Configuration:

```text
scopes/ada-command-center/domain/alarms
ada_command_center.domain.alarms
```

Reglas:

```text
Web -> Domain
Backend -> Domain
Domain -X-> Web
Domain -X-> Runtime/Persistence/Infrastructure
```

No usar `shared` como cajón genérico.

No duplicar DTOs equivalentes entre Web y Backend.

La extracción fue root replacement:
- `backend/alarms/core/definition.py` removido;
- `web/alarms/configuration/models.py` removido;
- sin aliases legacy.

## Alarm backend boundaries CURRENT

```text
backend/alarms/core
    Runtime/lifecycle/priority/management/deactivation contracts

backend/alarms/persistence
    durable Engine persistence/recovery

backend/alarms/materialization
    pure B.2 contracts and qualification inputs

backend/processes/alarms-runtime
    Runtime orchestration/execution

backend/processes/alarms-materialization
    PLANNED / not implemented
```

### Shared operational identity

`AlarmResolutionKey` vive en Alarm Core:

```text
alarm_configuration_revision
confirmed_tool_catalog_revision
```

Es compartido por Materialization y futuras superficies Runtime Adoption/Delivery sin hacer que Core dependa de Materialization.

No pertenece al authored Domain.

## Visibility boundary CURRENT

```text
Authored Domain:
VISIBLE | TRACE_ONLY

Runtime Core:
no visibility flag
no SHADOW disposition

Delivery:
visibility_mode
```

`TRACE_ONLY` participa de Runtime normal; Delivery decide publicación visible.

## Materialization contracts CURRENT

`backend/alarms/materialization` implementa contratos puros:

```text
AlarmConfigurationResolution
RuntimeAlarmConfiguration
DeliveryAlarmConfiguration
ResolvedDeliveryAlarm
ResolvedDeliveryMessage
ResolvedDeactivationPolicy
ResolvedVisualTarget
ResolvedVisualSubcomponentTarget
```

No contiene:
- resolver B.2 completo;
- I/O;
- stores;
- scheduler;
- process orchestration;
- Runtime Adoption;
- Live Delivery.

Qualification inputs implementados:

```text
ToolReconciliationQualification(green_tool_keys)
EvaluatorQualificationKey(family_key, evaluator_key)
EvaluatorQualificationCatalog(qualified_keys)
```

No copiar estados internos de Tool reconciliation ni evaluator runtime machinery.

## Configuration / Administration

Manager administra Source/Projection sólo para dominios que realmente son Configuration Sources.

```text
Source      = Local | Blob
Projection  = Local | Cosmos
```

Providers independientes.

Projection representa un release exacto; no reconstruir targets desde revision textual y no mantener contratos paralelos para transición.

## Application availability boundary

Invariante:

```text
APPLICATION EXISTENCE
!= CONFIGURATION EXISTENCE
!= INFRASTRUCTURE AVAILABILITY
!= DATA AVAILABILITY
```

Ausencia de Source/Projection/data puede ser estado funcional válido.

Errores contractuales deben permanecer diagnosticables.

## Command Center — próxima frontera

Después de cerrar Materialization Contracts + Qualification Inputs:

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
PLANNED / NEXT
```

El incremento debe permanecer pure backend:

```text
explicit inputs
-> deterministic validation/materialization
-> AlarmConfigurationResolution
```

No mezclar con acquisition, stores, Runtime Adoption, Live Delivery, Management Capture,
provenance migration ni process orchestration.
