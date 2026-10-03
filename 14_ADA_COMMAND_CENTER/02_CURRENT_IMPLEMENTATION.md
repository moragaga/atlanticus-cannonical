# ADA Command Center — Current Implementation

Estado: **CURRENT — GENERIC WEB CAPABILITIES + ALARM LIVE BACKEND VERTICAL IMPLEMENTED**

Checkpoint:

```text
moragaga/atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856
```

## Generic Application

Command Center mantiene su composition root Web con Home, Identity, Users, Profiles, Navigation, Manager, Tool Catalog y Alarm Configuration.

## Alarm configuration CURRENT

Command Center conserva authoring/validation/publication y la projection durable `alarm-configuration`.

## Alarm backend CURRENT

Físicamente bajo:

```text
scopes/ada-command-center/backend
```

Incluye:

```text
alarms/core
alarms/materialization
alarms/persistence
processes/alarms-materialization
processes/alarms-runtime
processes/alarms-modeler
processes/alarms-delivery
```

## Live backend CURRENT

```text
alarm-configuration Cosmos
→ Materialization READY
→ Runtime EFFECTIVE/CURRENT/FACTS
→ Modeler per-Tool snapshot
→ Delivery
→ alarm-live-projection Cosmos
```

Este flujo fue cualificado localmente con Cosmos real local y read-back.

## Web operational live surface

Aún no consume `alarm-live-projection` en el cierre de este hito.

Eso es el siguiente foco único.

## BLOCKED separado

El qualifier Web completo mantiene un blocker upstream por coexistencia de `ada.web.tools.*` y `ada.contracts.tools.*`.

No resolverlo dentro del live alarm consumer salvo que sea bloqueo directo demostrado.

## Engine extraction

La extracción física a un scope independiente sigue PLANNED y separada del próximo Web increment.
