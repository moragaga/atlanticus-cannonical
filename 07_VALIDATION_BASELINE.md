# Atlanticus — Validation Baseline

Estado: **CURRENT — ALARM LIVE BACKEND VERTICAL QUALIFIED LOCALLY 2026-10-03**

## Autoridad

```text
Implementation  moragaga/atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856
Canonical base  moragaga/atlanticus-cannonical@8efd59431754059c548ed1e5d1263533b81012cd
```

## Evidencia focal de Alarm

Reportada y observada durante el cierre:

```text
processes/alarms-runtime/tests                     PASS
processes/alarms-modeler/tests + delivery/tests  20 PASS
```

E2E físico local:

```text
Cosmos alarm-configuration           PASS
Materialization READY                PASS
Runtime EFFECTIVE                     PASS
NOTPII Parquet read                   PASS
Runtime ACTIVE/PREDOMINANT            PASS
Runtime CURRENT + FACTS               PASS
Modeler index + Tool snapshot         PASS
Delivery CURRENT_AVAILABLE            PASS
Delivery published_documents = 1      PASS
Delivery failed_tools = 0             PASS
Cosmos alarm-live-projection readback PASS
```

## Acredita

```text
exact artifact continuity from READY through Delivery
real local Cosmos read/write path
real Runtime source read from NOTPII dataset
Modeler eligibility/order baseline
per-Tool live snapshot generation
fixed live container publication
Tool-key connection resolution
Delivery work metric after successful publish
```

## No acredita

```text
monorepo-wide pytest
CI
independent Docker containers
Azure Cosmos / Key Vault / Entra
production qualification producer
CAROUSEL/QIQ scheduling
Modeler durable scheduler recovery
Management projection
History/Analytics
Command Center Web rendering of live projection
production resource provisioning/startup gates
```

## Important qualification note

El evaluator `mina/threshold` usado para el E2E es un **example evaluator**. El registry productivo de Runtime permanece vacío según su contrato/test vigente. El E2E lo inyectó mediante runner local controlado; no convertir esa prueba en registration productiva implícita.
