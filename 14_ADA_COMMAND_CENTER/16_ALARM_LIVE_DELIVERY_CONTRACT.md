# ADA Command Center — Alarm Live Delivery Contract

Estado: **CURRENT input contract / Live projection PLANNED**.

## Contract ownership update

Los schemas compartidos de salida Engine pertenecen a:

```text
ada-contracts-alarms==1.0.0
ada/contracts/alarms/schemas/
```

No usar las rutas históricas `backend/alarms/contracts/*.schema.json` como autoridad; esas copias fueron retiradas en `main@6725237...`.

## Current chain

```text
exact DeliveryAlarmConfiguration
+
EngineResolvedCurrentState CURRENT v1
        ↓
alarms-delivery input receiver
        ↓
[PLANNED] Live materialization
        ↓
[PLANNED] AlarmLiveProjection
```

Delivery input no es un segundo Engine y no consulta Tools ni reevalúa Rules.

## Exact alignment invariants

```text
READY != EFFECTIVE
same resolution_key
same exact artifact pin
source_key + result_id + manifest_sha256 + resolution_key
no fallback to latest READY
```

CURRENT publicado y Delivery Configuration deben corresponder al mismo EFFECTIVE exacto.

## Delivery configuration

Contiene metadata estática necesaria para presentation/delivery y visual targets resueltos. No debe incluir evaluator code, prioridad fuente para recomputar decisiones ni geometría/CSS.

## Live publication target

Cuando se implemente Live, Web recibe hechos ya resueltos y no recalcula:

- priority;
- routing;
- message precedence;
- deactivation capability;
- cause template semántica.

Visual target keys identifican elementos lógicos; Web decide representación física/visual.

## CURRENT vs PLANNED

CURRENT:

- Runtime CURRENT v1;
- FACTS v2;
- Delivery input receiver CURRENT-only;
- exact EFFECTIVE/READY checks;
- schemas en `ada-contracts-alarms`.

PLANNED / SEPARATE:

- `AlarmLiveProjection`;
- cause materialization definitiva;
- dispatch/escalation operacional;
- Management Capture;
- History/Analytics.

## Qualification

El gate actual de `ada-contracts` llegó más allá de Delivery y Web Alarm Configuration. Su bloqueo está en Command Center capability parity, no en este Live contract.
