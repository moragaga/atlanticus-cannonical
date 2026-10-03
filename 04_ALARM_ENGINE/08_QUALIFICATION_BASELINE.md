# Alarm Engine — Qualification Baseline

Estado: **CURRENT — LIVE BACKEND VERTICAL QUALIFIED LOCALLY**

## Authority

```text
implementation: atlanticus@38379979fad90e2c514a2d56f3aa3889ceb71856
canonical base: atlanticus-cannonical@8efd59431754059c548ed1e5d1263533b81012cd
Python workspace: 3.14.2
```

## Test evidence

```text
processes/alarms-runtime/tests                     PASS
processes/alarms-modeler/tests + delivery/tests  20 PASS
```

Los resultados son locales aportados por el usuario; no equivalen a CI remota.

## Physical E2E evidence

VERIFIED:

```text
Cosmos database reachable
alarm-configuration container created/used
alarm-live-projection container created/used
Materialization READY produced
Runtime exact artifact adopted EFFECTIVE
NOTPII daily dataset consumed
Runtime alarm evaluated ACTIVE
priority disposition PREDOMINANT
Tool assignment present
CURRENT + FACTS written
Modeler index + Tool snapshot written
Delivery CURRENT_AVAILABLE
published_documents = 1
failed_tools = 0
Cosmos read-back matched modeled snapshot
```

## Evaluator qualification note

El E2E usó `mina/threshold`, que permanece example-only.

El registry productivo de Runtime conserva `contracts=()` y los tests lo exigen. La prueba inyectó el evaluator mediante un runner local controlado. No promoverlo a productivo por inferencia.

## Local qualification artifact note

El `qualification.json` usado fue evidencia local construida para coincidir exactamente con la projection/release del E2E. No representa todavía el productor productivo definitivo de qualification.

## UNVERIFIED

```text
CI
multi-container Docker
Azure Cosmos
Key Vault
Entra
production qualification producer
full scheduler recovery
Web live rendering
Management/Analytics
```
