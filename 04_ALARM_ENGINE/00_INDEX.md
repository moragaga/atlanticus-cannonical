# Alarm Engine — Index

Estado: **CURRENT DOMAIN / RUNTIME DATA-INTEGRATION BLOCKED / WEB ALARM SURFACE NEXT**

## Ownership CURRENT

Los contratos de dominio, persistence, materialization, Modeler y Delivery del Alarm Engine permanecen vigentes.

`ada-contracts-alarms` conserva ownership de contratos compartidos de configuración/publicación.

El owner operacional CURRENT documentado sigue siendo `ada-command-center-alarms-core` hasta completar una migración explícita.

## Runtime integration status

`processes/alarms-runtime` permanece canónicamente:

```text
BLOCKED
```

Causa CURRENT documentada:

```text
Operational Data legacy consumer contract was removed.
Alarm Runtime still imports removed legacy input contracts.
```

Este bloqueo es intencional.

No restaurar legacy ni introducir shims para reactivar Runtime.

## Web path — PLANNED / NEXT IN THIS TRACK

Antes de migrar el engine, el siguiente incremento debe preparar la superficie Web de alarmas:

```text
first reusable alarms-flow component/card
integrate alarm-management
integrate alarm-status
exercise operational header with all intended surfaces
freeze Web consumption boundary
```

No implementar engine migration dentro del mismo incremento.

## Engine target — PLANNED AFTER WEB FOUNDATION

Dirección acordada:

```text
operational Alarm Engine
→ target distribution/package identity: ada-alarm-engine
```

La migración debe comenzar con inventario y diseño, no con rename mecánico.

Debe decidir explícitamente:

```text
what remains in ada-contracts-alarms
what belongs to ada-alarm-engine
what historical fields do not add value
which removals are compatible with frozen domain semantics
how Runtime migrates to DataInputSpec/DataInputContext
```

No modificar `AlarmDefinition` como efecto lateral de corregir Operational Data integration.

## Target data-input migration

La dirección canónica previa continúa:

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

La forma exacta debe debatirse antes de implementar.

## Historical pipeline evidence

El pipeline históricamente cualificado:

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

permanece como evidencia histórica.

No describirlo como ejecutable CURRENT mientras Runtime siga bloqueado por su integración de datos.

## Frozen Web boundary

Alarmas no siguen la regla:

```text
configured definition
→ permanent visible component
```

La presencia visible se deriva de lifecycle/projection.

Web no debe leer WAL ni Runtime CURRENT directamente como contrato final de visualización.

## Next

```text
ADA-WEB-ALARM-SURFACE-FOUNDATION
```

Después:

```text
ADA-ALARM-ENGINE-MIGRATION
```
