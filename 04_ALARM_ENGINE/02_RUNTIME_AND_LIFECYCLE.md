# Alarm Engine — Runtime and Lifecycle

Estado: **CURRENT DOMAIN/LIFECYCLE CONTRACTS — EXECUTABLE RUNTIME BLOCKED BY OPERATIONAL DATA CUTOVER**

## Boundary

Los contratos de lifecycle y verdad operacional de Alarm permanecen CURRENT.

El proceso ejecutable `alarms-runtime` está BLOCKED porque su integración de datos todavía usa contratos Operational Data removidos.

No interpretar este bloqueo como supersession del dominio Alarm.

## Cycle boundary — CURRENT

`reduce_group_cycle` recibe estado durable de grupo, `cycle_at`, `PlannedAlarm`, evaluaciones de ciclo, closures de configuración, acciones Management, deactivation y factories/resolvers explícitos. No depende de estado global.

## Orden lógico de Core CURRENT

```text
1. validar cycle/configuration inputs
2. indexar PlannedAlarm y evaluaciones
3. preparar Management/deactivation inputs
4. resolver lifecycle físico/técnico y occurrences
5. construir siguiente GroupLifecycleState
6. finalizar Management/deactivation
7. resolver routing
8. resolver priority
9. emitir GroupLifecycleDecision
```

## Priority CURRENT

```text
PREDOMINANT
ECLIPSED
CASCADE_SUPPRESSED
DEACTIVATED
```

No reintroducir `SHADOW` ni `delivery_enabled` a Runtime Core.

## Reconfiguration/adoption

READY/EFFECTIVE, WAL, exact artifact pin y sesión fijada permanecen contratos CURRENT del Alarm Engine.

## Operational Data integration — SUPERSEDED / BLOCKED

La integración anterior usaba:

```text
DataRequirement
DataRequirementPlanner
DataLoadPlan
DataRuntimeContext
source + partition lookup
```

Esos contratos fueron removidos de Operational Data.

Estado:

```text
old Alarm Runtime data integration = SUPERSEDED
current alarms-runtime executable path = BLOCKED
new Alarm data-input migration = PLANNED
```

No crear adapters ni restaurar aliases.

La migración futura debe resolver inputs por identidad local y consumir `DataInputContext`, sin alterar incidentalmente `AlarmDefinition` ni las reglas de lifecycle.

## OPEN separado

```text
Alarm Runtime data-input migration
reappearance contract reconciliations already open
advanced Modeler scheduling
production qualification
```
