# Atlanticus — Current State

Estado: **CURRENT — ALARM BACKEND VERTICAL MATERIALIZATION → RUNTIME → MODELER → DELIVERY → COSMOS CLOSED / VERIFIED**

## Autoridad

```text
Implementation
moragaga/atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856

Canonical base before replacement
moragaga/atlanticus-cannonical@8efd59431754059c548ed1e5d1263533b81012cd
```

## CLOSED / VERIFIED en este hito

El backend de alarmas quedó probado físicamente end-to-end en local:

```text
Alarm Configuration Cosmos
    container alarm-configuration
    PK /partition_key
        ↓
Materialization READY
        ↓
Runtime EFFECTIVE exact pin
        ↓
NOTPII Parquet input
        ↓
Runtime CURRENT v1 + FACTS v2
        ↓
Alarm Modeler current projection
        ↓
Alarm Delivery
        ↓
Cosmos alarm-live-projection
        ↓
read-back del mismo snapshot
```

La prueba real produjo una alarma `ACTIVE`, `PREDOMINANT`, asignada al Tool configurado, proyectada en `operator_pool` y `operator_view`, publicada y leída de Cosmos.

## Alarm backend CURRENT físico

Permanece bajo:

```text
scopes/ada-command-center/backend/
```

Packages/processes CURRENT:

```text
alarms/core
alarms/materialization
alarms/persistence
processes/alarms-materialization
processes/alarms-runtime
processes/alarms-modeler
processes/alarms-delivery
```

La extracción física a un scope independiente sigue PLANNED; no fue parte de este cierre.

## Materialization CURRENT

Sigue produciendo la pareja exacta:

```text
RuntimeAlarmConfiguration
DeliveryAlarmConfiguration
```

Ambas comparten el mismo `resolution_key` y el mismo exact artifact pin.

La decisión previa que exigía un `ModelerConfiguration` separado antes de implementar Modeler queda **SUPERSEDED / REFINED** para el baseline actual: el Modeler implementado consume `RuntimeAlarmConfiguration + DeliveryAlarmConfiguration` del mismo READY exacto.

Un split posterior sólo debe introducirse si el scheduler completo necesita un contrato independiente real.

## Runtime CURRENT

Runtime conserva verdad operacional y publica:

```text
runtime/output/current/latest.json
runtime/output/facts/facts-<hash>.json
runtime/output/state/facts-export-cursor.json
```

CURRENT v1 incluye evaluación, prioridad, occurrences/episodes, assignments, holds y estado operacional actual.

FACTS v2 conserva hechos durables de commits.

## Modeler CURRENT — baseline de proyección

El proceso `alarms-modeler` está implementado.

Lee:

```text
runtime/state/effective-head.json
runtime/output/current/latest.json
exact READY runtime.json + delivery.json
```

Valida el exact artifact pin y publica:

```text
modeler/output/current/index.json
modeler/output/current/tools/<sha256(tool_key)>/latest.json
```

El snapshot por Tool contiene:

```text
alarms
operator_pool
operator_view
artifact_ref
snapshot_timestamp
meta
sha256
```

Elegibilidad CURRENT:

```text
Runtime evaluation == ACTIVE
Runtime priority.disposition == PREDOMINANT
Delivery alarm active
VisibilityMode.VISIBLE
Runtime assignment incluye el Tool
visual_target incluye el Tool
```

Orden CURRENT:

```text
priority_order
started_at
alarm_identity
occurrence_id
```

`operator_view` toma los primeros 6 elementos de `operator_pool`.

Esto **no** implementa todavía CAROUSEL/QIQ, timers, fairness ni durable scheduler state.

## Delivery CURRENT

Delivery ya no consume Runtime directamente.

Lee exclusivamente el head actual del Modeler, valida:

```text
index checksum
snapshot checksum
source_key
artifact_ref
snapshot_timestamp
EFFECTIVE exact pin
exact READY availability
```

y publica documentos ya modelados hacia Cosmos.

Contrato CURRENT:

```text
container fijo: alarm-live-projection
connection selection: por tool_key
config/connections.json: tool_key -> nombres de variables endpoint/database/credential
```

El contenedor no se configura por Tool.

## Live Projection CURRENT

Documento CURRENT:

```text
document_type = ada_alarm_projection_snapshot
schema_version = 1
id = alarm_projection_snapshot:<tool-hash-prefix>
partition key contractual = /tool_key
```

`ranking` no existe; `priority_order` es la única prioridad ordinal proyectada.

## Qualification observada

VERIFIED local:

```text
processes/alarms-runtime/tests                     PASS
processes/alarms-modeler/tests + delivery/tests  20 PASS
real Cosmos configuration projection             PASS
real Materialization READY                       PASS
real Runtime EFFECTIVE/CURRENT/FACTS             PASS
real NOTPII read                                 PASS
real Modeler projection                          PASS
real Delivery publish                            PASS
Cosmos read-back                                 PASS
```

No equivale a CI, Docker separado ni Azure.

## OPEN separado

```text
full CAROUSEL/QIQ scheduler
rotation duration 90 vs 120 s
Modeler durable scheduler state/checkpoint
artifact A -> B scheduler-state migration
dynamic effective cause materialization
Management projections/lifecycle completion
History/Analytics
Web consumption of alarm-live-projection
Alarm Engine physical extraction
Command Center full Web qualifier blocker on Tool type duplication
production qualification producer
production Azure/Entra/Docker qualification
Python 3.14.7/Trixie migration
```

## NEXT único

```text
ADA-COMMAND-CENTER-ALARM-LIVE-WEB-CONSUMER
```

Objetivo: hacer que Web consuma `alarm-live-projection` sin recalcular prioridad, routing ni scheduling.
