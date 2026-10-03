# Atlanticus — Current State

Estado: **CURRENT — COMMAND CENTER CAPABILITY PARITY CLOSED; ALARM ENGINE EXTRACTION DESIGN NEXT**

## Autoridad

```text
Implementation
moragaga/atlanticus@346e7ac7ba7c21eede8b524613a6adee7e839e55

Canonical before replacement
moragaga/atlanticus-cannonical@19fe30dcc2f34dbe7a0c4615c188409ace2089b8
```

## CLOSED / VERIFIED relevante

```text
ADA-TOOL-SCOPED-SOURCE-OWNERSHIP
ADA-USERS-GLOBAL-IDENTITY
ADA-TOOL-USER-MEMBERSHIP
ADA-USERS-RUNTIME
ADA-USERS-RECOVERY-SNAPSHOT
MASTER-PROJECTION-USERS-REPLACE

KPI-NAMED-CONNECTIONS
KPI-REGISTRY-MATERIALIZATION
KPI-LATEST-MULTI-TOOL-DELIVERY
KPI-HISTORIAN-ROLLING-READ-MODEL
KPI-TIMESERIES-MULTI-TOOL-DELIVERY
KPI-HISTORY-DATASET-BOUNDARY

COMMAND-CENTER-USERS-PROFILES-NAVIGATION-MANAGER-PARITY
COMMAND-CENTER-WEB-LOCK-NORMALIZATION
```

## Command Center capability parity CURRENT

`ada-command-center` consume ahora las capabilities genéricas actuales sin API legacy de Users.

```text
UsersRegistryStore
ToolMembershipStore
UsersRuntimeStore
UsersDirectoryReader

UsersAdministrationService(
    registry=...,
    memberships=...,
    profiles=...,
    directory=...,
)
```

Composición durable:

```text
Global Users Registry
    <application>/users/users.json.gz

Tool Membership
    <application>/<tool>/users/memberships.json.gz

Users Runtime
    Cosmos users-runtime por Tool

Users Recovery
    <application>/<tool>/users/recovery/...
```

Navigation usa la autoridad genérica `NAVIGATION_SOURCE_KEY` y la semántica `PUBLIC | RESTRICTED`.

Manager principal/runtime binding sigue el contrato de `RuntimeUser`, con override root y local sólo bajo ambiente local confiable.

Users permanece una operación especial de recovery en Master Projection; no es un `ProjectionDomain` ordinario.

## Qualification focal del cierre

Reportado y verificado en el hito:

```text
ada-command-center-configuration-manager   31 passed
ada-command-center-generic-application     12 passed
ada-command-center-web-tool-catalog-manager 8 passed

Ruff focal                            PASS
```

Los cuatro lockfiles alcanzados por el qualifier fueron normalizados y posteriormente integrados en `atlanticus:main`:

```text
web/application/ada-command-center-generic-application/uv.lock
web/tools/catalog-manager/uv.lock
web/tools/catalog/uv.lock
web/tools/discovery-cosmos/uv.lock
```

Para los cuatro:

```text
uv lock --check     PASS
uv sync --locked    PASS
```

## BLOCKED separado

```text
COMMAND-CENTER-FULL-WEB-QUALIFIER
```

El qualifier ya no está bloqueado por Users / Profiles / Navigation / Manager.

El bloqueo observado está en la frontera Tools:

```text
catalog
    7 failed / 5 passed

discovery-cosmos
    1 failed / 33 passed
```

Causa observada:

```text
ToolCatalogEntry espera:
    ada.contracts.tools.ToolConfigurationKind
    ada.contracts.tools.ToolStructure

ADA ToolConfiguration actual produce:
    ada.web.tools.ToolConfigurationKind
    ada.web.tools.ToolStructure
```

Los valores documentales pueden coincidir, pero las clases Python no son idénticas.

Tratamiento:

```text
NO modificar ADA dentro de este hito
NO relajar ToolCatalogEntry
NO introducir adapters/shims en Command Center
registrar el bloqueo y continuar en el frente correspondiente cuando se abra ADA
```

## Alarm backend CURRENT

Físicamente continúa bajo:

```text
scopes/ada-command-center/backend/
```

Incluye:

```text
alarms/core
alarms/materialization
alarms/persistence
processes/alarms-materialization
processes/alarms-runtime
processes/alarms-delivery
```

Hipótesis de trabajo acordada para el próximo chat:

```text
ese backend constituye en esencia el Alarm Engine;
la extracción debe conservarlo como unidad y remover/invertir
las capas que todavía apuntan a Command Center Web.
```

Esta hipótesis es **PROPOSED / PLANNED**, todavía no una extracción implementada.

## NEXT

```text
ADA-ALARM-ENGINE-EXTRACTION-DESIGN
```

Primero inventario/dependency graph. Implementación sólo después de congelar contratos y ownership.
