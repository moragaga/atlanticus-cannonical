# Web Platform — Open Items

Estado: **CURRENT / NAVIGATION LOCAL CLOSED / REAL DISTRIBUTION AND PROVIDERS OPEN**

Checkpoint del delta: `moragaga/atlanticus@a6061ffed59c8b04e64b0a7fdc17050ef463c850`.

## CLOSED / CURRENT

```text
ADA-STORAGE-NAMESPACE
TOOL-PROJECTION-PERSISTENCE
TOOL-PERSISTENCE-RESILIENT-COMPOSITION
ADA-WEB-KPI-COLLECTOR-CAPABILITY
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
ADA-GENERIC-MANAGER-LOCAL-INTEGRATION
ADA-GENERIC-MANAGER-DURABLE-ADAPTER-COMPOSITION
ADA-GENERIC-MANAGER-RESOURCE-CLI
ADA-GENERIC-INTERNAL-COSMOS-CONTAINER-NAMES
NAVIGATION-OPERATIONAL-AUTHORIZATION-INTEGRATION
NAVIGATION-MANAGER-LOCAL-RECOVERY
NAVIGATION-LOCAL-PUBLISH-PROJECT-CONSUME (VERIFIED MANUAL)
NAVIGATION-CLIENT-CORRECTION (USER-REPORTED FUNCTIONAL)
```

## Abiertos separados — ninguna implementación autorizada por este cierre

```text
ADA-GENERIC-DOCKER-REAL-PERSISTENCE-QUALIFICATION   PLANNED / UNVERIFIED
ADA-GENERIC-WEB-ARTIFACT-DISTRIBUTION-QUALIFICATION  PLANNED / NEXT
FIRST REAL TOOL GOLDEN PATH                       PLANNED / OPEN
BLOB-PROVISIONING-PARITY                         OPEN
APPLICATION-RESOURCE-PLAN GLOBAL                 PLANNED / OPEN
PRODUCTION-IDENTITY-PROVIDER / ENTRA             PLANNED / UNVERIFIED
FULL WEB READINESS / RESOURCE ORCHESTRATION      PLANNED / OPEN
PYTHON-METADATA-ALIGNMENT 3.14.7 / 3.14.2         CONFLICT / PLANNED
CI REMOTE / FULL RUFF WORKSPACE / MONOREPO TESTS UNVERIFIED
```

La antigua etiqueta `NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT BLOCKED` está
**SUPERSEDED**: el consumidor actual se implementó y el usuario verificó el recorrido
local. No inferir producción cualificada por esa corrección.

La antigua observación estática de dos `AccessRuntime` se mantiene como hallazgo
histórico pendiente de revalidación, no como defecto runtime demostrado.

## Siguiente foco único

`ADA-GENERIC-WEB-ARTIFACT-DISTRIBUTION-QUALIFICATION`.
Revisar el generador realmente existente antes de decidir si se necesita código adicional;
identificar la entrega mínima transportable, el cierre de dependencias, el `.env.detail`,
el arranque fuera del checkout y los límites del host productivo.

No incorporar aquí refactor/backend, Tool real, Entra o nueva UI.

## Atajos prohibidos

No legacy, aliases, shims, múltiples contratos equivalentes, hardcoded Tool configuration,
nombres de contenedores Cosmos en `.env`, polling Cosmos inline desde browser, fallback a
Source en runtime, o permisos Navigation como sustituto del control de Manager.
