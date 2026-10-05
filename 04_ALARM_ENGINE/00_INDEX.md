# Alarm Engine — Index

Estado: **CURRENT DOMAIN / RUNTIME DATA-INTEGRATION BLOCKED**

## Ownership

Los contratos de dominio, persistence, materialization, Modeler y Delivery del Alarm Engine permanecen vigentes.

## Runtime integration status

`processes/alarms-runtime` está actualmente:

```text
BLOCKED
```

Causa:

```text
Operational Data legacy consumer contract was removed.
Alarm Runtime still imports:
- DataRequirement
- DataLoadPlan
- DataRequirementPlanner
- DataRuntimeContext
```

Este bloqueo es intencional. No restaurar legacy ni introducir shims para reactivar Runtime.

## Target migration

PLANNED, en un incremento separado:

```text
Alarm evaluator/input contract
    ↓
DataInputSpec
    ↓
DataInputPlanner
    ↓
DataInputLoader
    ↓
DataInputContext
```

La forma exacta del contrato Alarm deberá debatirse antes de implementar; no modificar `AlarmDefinition` como efecto lateral.

## Pipeline histórico previamente cualificado

El baseline previamente cualificado:

```text
Alarm Configuration projection
    ↓
Materialization READY
    ↓
Runtime EFFECTIVE
    ↓
Runtime CURRENT + FACTS
    ↓
Modeler
    ↓
per-Tool AlarmProjectionSnapshot
    ↓
Delivery
    ↓
alarm-live-projection
```

permanece como evidencia histórica, pero no debe describirse como ejecutable CURRENT mientras Runtime siga importando contratos retirados.

## NEXT del Project

Alarm no es el próximo foco.

```text
ATLANTICUS-DISTRIBUTION-AND-TOOLING-FINAL-QUALIFICATION
```
