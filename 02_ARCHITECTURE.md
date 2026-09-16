# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA.

## Planos principales

### Platform

Capacidades transversales:

- backend;
- connectivity;
- integrations;
- web.

`backend/` representa backend jobs y capacidades propias de esos jobs.

`web/` es frontera de primer nivel para Flask/Dash, JavaScript/CSS, composición Web, server-side Python con responsabilidad Web y capabilities Web reutilizables.

Connectivity es dual-use y no adquiere ownership funcional.

### Configuration / Administration

Manager administra configuración, authoring, validation, publication, history y projection actions.

Source genérico pertenece a:

```text
web/capabilities/source/
```

Projection genérica exact-release pertenece a:

```text
web/capabilities/projection/core
```

Manager consume estos contratos genéricos directamente. No mantiene una arquitectura paralela `legacy` vs `exact`.

### Operational Data

Operational Data conserva ownership separado para sources, producers, processes, planner y materialization.

### ADA Runtime

ADA Generic compone la experiencia operacional y consume capacidades Atlanticus.

ADA-specific authorization puede consumir/extender contratos genéricos, pero no convertirse en dependencia del core Atlanticus.

## Manager vs ADA Generic

```text
Manager      = administrar configuración
ADA Generic  = consumir configuración y materializar experiencia operacional
```

No comparten ownership de shell/header.

## Configuration vs Data

```text
CONFIGURATION DETERMINES EXISTENCE
DATA DETERMINES STATE
```

## Source vs Projection

Source y Projection son responsabilidades separadas.

```text
Source     = Local | Blob
Projection = Local | Cosmos | provider equivalente
```

Projection representa un `SourceReleaseRef` concreto.

Source current nunca se determina desde Cosmos.

## Contrato único de Manager

Cada `ManagerModule` declara:

```text
SourceKey
source_service
source_reader_service
projection_service
draft_validation_service
source_history_service | None
```

No existe una segunda familia `exact_*`.

### Source

```text
SourceReaderWorkflow
SourcePublicationWorkflow
SourceHistoryWorkflow
```

Todos transportan modelos de `source/core`.

### Projection

Manager consume:

```text
get_status(source_key)
select_current_target(source_key)
project(ProjectionTarget)
```

No existe adapter Manager hacia una identidad textual de revisión.

## Workspace genérico

`ManagerWorkspace` conserva:

```text
owner
payload local
SourceSnapshot como BASE
local revision
base payload revision
saved_at
```

Reglas:

- local revision identifica payload local;
- Source release identity permanece en `SourceSnapshot`;
- concurrency token no se convierte en release identity;
- schema vigente = `2`;
- no hay parser/shim legacy para workspace anterior.

## Navigation CURRENT

Navigation ya es consumidor directo del contrato genérico.

Fronteras:

```text
web/capabilities/navigation/core
    dominio/runtime reusable

web/capabilities/navigation/configuration
    codec/source service
    projection builder
    editor/domain validation
    web editor payload integration

web/capabilities/navigation/projection-local
    ProjectionStore local

web/capabilities/navigation/projection-cosmos
    ProjectionStore Cosmos

web/compositions/navigation-manager
    binding explícito Navigation <-> Manager
```

Local:

```text
LocalSourceStore
 -> NavigationSourceService
 -> Manager Source workflows
 -> SourceProjectionService
 -> LocalNavigationProjectionStore
```

Azure:

```text
BlobSourceStore
 -> NavigationSourceService
 -> Manager Source workflows
 -> SourceProjectionService
 -> CosmosNavigationProjectionStore
```

No existe `navigation/cosmos` como runtime store independiente porque Navigation no tiene una responsabilidad runtime equivalente a Users.

## Reglas congeladas para Navigation

```text
LEGACY                      REMOVE
ADAPTERS / SHIMS / ALIASES FORBIDDEN
DOBLE CONTRATO              FORBIDDEN
revision -> ProjectionTarget reconstruction REMOVE
expected_source_revision    REMOVE
```

Navigation no recibe una arquitectura especial.

## Consumer boundary

El cierre de Navigation no demuestra alineación automática de otros consumers.

`users-manager` presenta una desalineación observable con el contrato Manager CURRENT. Esa desalineación debe validarse como frente independiente antes de decidir su solución.

Tools, KPI Configuration y KPI Definition conservan su estado anterior hasta inspección propia.

## Fronteras futuras

```text
USERS-MANAGER-ALIGNMENT-VALIDATION                 PLANNED / NEXT
TOOLS-MANAGER-GENERIC-CONSUMER-CUTOVER             PLANNED
KPI-CONFIG-MANAGER-GENERIC-CONSUMER-CUTOVER       PLANNED
KPI-DEFINITION-MANAGER-GENERIC-CONSUMER-CUTOVER   PLANNED
MANAGER-CONSUMER-GLOBAL-QUALIFICATION              BLOCKED
```

No reabrir Manager core ni Navigation para resolver otro consumer.
