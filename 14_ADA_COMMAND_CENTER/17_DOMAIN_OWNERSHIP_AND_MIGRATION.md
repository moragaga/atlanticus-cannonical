# ADA Command Center — Domain Ownership and Migration

Estado: **CURRENT — LIVE PIPELINE IMPLEMENTED IN COMMAND CENTER BACKEND; PHYSICAL ENGINE EXTRACTION PLANNED**

Checkpoint:

```text
atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856
```

## Shared Tool contracts — CURRENT

```text
scopes/ada-contracts/tools
ada-contracts-tools==1.0.0
```

## Shared Alarm contracts — CURRENT

```text
scopes/ada-contracts/alarms
ada-contracts-alarms==1.0.0
```

Poseen configuration models/snapshot/shared schemas.

## Command Center ownership — CURRENT

Command Center conserva:

```text
Alarm authoring
semantic validation
Tool/reference validation
routing/visual validation
publication
Web/Manager surfaces
```

## Backend Engine — CURRENT physical location

```text
scopes/ada-command-center/backend/alarms/core
scopes/ada-command-center/backend/alarms/materialization
scopes/ada-command-center/backend/alarms/persistence
scopes/ada-command-center/backend/processes/alarms-materialization
scopes/ada-command-center/backend/processes/alarms-runtime
scopes/ada-command-center/backend/processes/alarms-modeler
scopes/ada-command-center/backend/processes/alarms-delivery
```

## Physical extraction — PLANNED / SEPARATE

Todo este backend sigue siendo candidato conceptual a Alarm Engine independiente.

No moverlo durante el próximo Web increment.

Cuando se retome:

```text
remove/invert backend -> Web dependencies
rehome residual domain/alarms ownership
rename cleanly
no re-export packages
no temporary aliases
no parallel engine copies
```

## Materialization ownership refinement

El live baseline funciona con `RuntimeAlarmConfiguration + DeliveryAlarmConfiguration`. No crear un `ModelerConfiguration` sólo por simetría; introducirlo únicamente si full scheduling demuestra responsabilidad independiente.

## Current next boundary

El próximo foco no es extracción. Es Command Center Web consumiendo el `alarm-live-projection` ya publicado.
