# Atlanticus — Current State

Estado: **CURRENT — ADA DISTRIBUTED RUNTIME CLOSED; TOOL-SCOPED CONFIGURATION CUTOVER NEXT**

## Autoridad

```text
Implementation
moragaga/atlanticus@38bcd8c5607d67f999e2bc4bf9dbf176c8340588

Decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical inspected before replacement
moragaga/atlanticus-cannonical@0a2ff691d8bb9a4bfe743cadb1cacb9c83d865c7
```

Git permanece **SOLO LECTURA**.

## Estado transversal no modificado por este hito

```text
Shared Master Projection                    CURRENT
StorageNamespace generic                    CURRENT
Source Core / Local / Blob                 CURRENT
Command Center product work                separate
Alarm Engine / Command Center alarm work   separate
KPI Engine backend recovery work           separate
```

Este cierre no recalifica esos dominios salvo donde se los enumera explícitamente.

## CLOSED / VERIFIED

```text
CURRENT-HEAD-DISTRIBUTION-REGENERATION
ADA-LOCAL-COSMOS-DATA-EXPLORER
ADA-DISTRIBUTED-LINUX-RUNTIME-SMOKE
ADA-CONSUMER-REPOSITORY-RUNTIME
```

### Runtime distribuido ADA

Evidencia reportada desde un repositorio consumidor aislado:

```text
Docker image build                  COMPLETED
image                               ada-generic:compose-local
Python image                        python:3.14.2-slim-bookworm

Azurite                             RUNNING
Cosmos Emulator                     RUNNING
Cosmos Data Explorer                HTTP 200 / port 1234

resource preparation
  Blob container dataproduct        CREATED
  Cosmos database cosmosdb-ada      CREATED
  six Cosmos containers             CREATED

Web                                 healthy
/health/live                        HTTP 200
/health/ready                       HTTP 200
application version                 0.2.26
```

`/health/ready` continúa entregando `checks: {}`; el endpoint responde pero no demuestra checks funcionales de dependencias.

## VERIFIED findings durante configuración real

- `StorageNamespace(application_namespace, scope_namespace)` produce `conciencia_situacional/<tool>`.
- La implementación durable actual usa `application_source` para Navigation, Profiles, ADA Access y Operational.
- Tools, KPI Registry y KPI Definitions usan `tool_source`.
- Users Registry durable es global bajo `<application_namespace>/users/users.json.gz`.
- `UserRecord` global CURRENT contiene `profile_key` y `enabled`.
- Tool Projection CURRENT contiene `tool_key` y `display_name`.
- KPI Registry Projection CURRENT registra dependency exacta hacia Tool Projection.
- El header ya soporta `tool_display_name`, pero el worker lo resuelve al bootstrap; refresco dinámico tras reproyección no está implementado.
- Time Status ya modela PI/Dispatch, pero el circuito runtime que alimenta sus timestamps sigue incompleto.

## DECIDED / PLANNED

### Tool-scoped configuration cutover

Bajo:

```text
ADA_APPLICATION_NAMESPACE=conciencia_situacional
ADA_TOOL_NAMESPACE=<tool>
```

la configuración específica de la Tool debe quedar bajo:

```text
conciencia_situacional/<tool>/...
```

Incluye:

```text
tools
profiles
navigation
ada-access
operational
tool-user-membership
kpis
kpi-definitions
tool users recovery snapshot
```

El registro global `users` queda como identidad compartida y pierde `profile_key` y `enabled`.

### users-runtime por Tool

Cosmos pertenece operativamente a una Tool.

`users-runtime` será la autoridad de lectura de sesión de esa Tool y expondrá un snapshot completo con:

```text
identity
enabled
resolved profile
operational
```

La estructura `operational` siempre existe; valores no informados se representan con `null`.

Access no se duplica dentro de cada usuario; se mantiene como contrato `profile_key -> access_keys`.

### Navigation

Debe distinguir explícitamente:

```text
PUBLIC
RESTRICTED
```

Semántica aceptada:

```text
PUBLIC
    acceso ordinario según aplicación

RESTRICTED + allowed_profiles=[]
    sólo root/local

RESTRICTED + allowed_profiles=[...]
    perfiles seleccionados + root/local
```

Root/local continúan implícitos y no son opciones editables normales.

### KPI Registry

La proyección/materialización de KPI Registry debe exponer `tool_key` derivado de Tool Projection para consumo de Delivery.

No duplicar `display_name` como autoridad editable del Source KPI.

## BLOCKED

```text
ADA-DURABLE-CONFIGURATION-RECOVERY
```

El recovery destructivo Blob → Cosmos se detiene hasta implementar el cutover Tool-scoped/Users mínimo. No tiene sentido certificar recovery sobre un ownership que ya fue descartado.

## OPEN no bloqueante

```text
host sync macOS / Python 3.14.2
    BLOCKED por rcssmin==1.2.2 sin wheel macOS CPython 3.14 bajo --only-binary

/health/ready checks
    vacío

Tool runtime hot refresh
Time Status runtime integration
KPI Delivery tool context consumption
Python 3.14.7 / Trixie
production Azure / Entra
Command Center distributed runtime
Alarm integration
```

## Unique next focus

```text
ADA-TOOL-SCOPED-CONFIGURATION-AND-USER-RUNTIME
```

Resolver sólo esa raíz, regenerar/distribuir y volver a la configuración funcional ADA. No mezclar UI/Alarmas durante ese incremento.
