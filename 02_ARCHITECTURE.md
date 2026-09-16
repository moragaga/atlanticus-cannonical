# Atlanticus — Architecture

Estado: **CURRENT**

## Regla principal

Atlanticus es plataforma modular reusable.

ADA consume Atlanticus.

El núcleo genérico de Atlanticus no depende de ADA.

## Ownership y scopes

`scopes/` contiene composiciones y capacidades específicas de un producto/proyecto cuando corresponde.

Una capability bajo `scopes/ada` puede consumir infraestructura genérica Atlanticus sin transferir su ownership al core genérico.

Regla CURRENT:

```text
Atlanticus generic infrastructure
    Source / Projection / Manager / Navigation / Users / Profiles / ...

ADA-specific capabilities
    Tools / KPI Configuration / KPI Definition / future Access / ...
```

No generalizar una capability sólo porque reutiliza contratos genéricos.

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

Los dominios ADA-specific consumen estos contratos sin moverse al core.

### Operational Data

Operational Data conserva ownership separado para sources, producers, processes, planner y materialization.

### ADA Runtime

ADA Generic compone la experiencia operacional y consume capacidades Atlanticus y contratos ADA-specific ya resueltos.

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

Projection representa un `SourceReleaseRef` concreto mediante `ProjectionTarget`.

Source current nunca se determina desde Cosmos.

`ProjectionTarget.dependencies` representa dependencias semánticas exactas entre projections cuando existen realmente.

No existe un orden global obligatorio de todas las proyecciones.

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
- no reconstruir `ProjectionTarget` desde revision.

## Navigation CURRENT

```text
NAVIGATION-GENERIC-CONFIGURATION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Users CURRENT

```text
USERS-MANAGER-GENERIC-CONTRACT-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## Tools CURRENT

Ownership:

```text
scopes/ada/web/tools
```

Tool Configuration conserva semántica ADA y consume infraestructura genérica Source/Projection.

```text
TOOLS-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

## KPI Configuration CURRENT

Ownership:

```text
scopes/ada/web/kpis/configuration
```

KPI Configuration conserva semántica ADA y consume directamente Source/Projection genéricos.

```text
KPI-CONFIG-GENERIC-SOURCE-PROJECTION-CUTOVER
CLOSED / VERIFIED / CURRENT
```

Dependencia exacta:

```text
Tool ProjectionTarget
        ↓ dependency
KPI Configuration ProjectionTarget
```

El catálogo de destinos conserva semántica de dominio; la procedencia exacta de Tool se transporta en `KpiDestinationCatalogSnapshot.projection_target`.

No forman parte del contrato CURRENT:

```text
private KPI lifecycle
private source revision identity
private projection revision identity
tool_projection_revision as dependency identity
expected_source_revision
revision -> ProjectionTarget reconstruction
```

## Consumer cutover strategy CURRENT

Los dominios de Configuration se migran primero hasta su contrato final aunque el consumer `ada-configuration-manager` quede temporalmente desalineado.

No crear compatibilidad para sostener el consumer durante la transición.

Orden:

```text
Tools Source/Projection                    CLOSED / CURRENT
KPI Configuration Source/Projection       CLOSED / CURRENT
KPI Definition Source/Projection          PLANNED / NEXT
ADA Configuration Manager final cutover   BLOCKED
Global regression                         BLOCKED
```

## KPI dependency semantics

KPI Configuration depende semánticamente del exact Tool Projection target.

KPI Definition depende semánticamente de KPI Configuration Projection.

Las identidades de estas dependencias usan el contrato genérico de Projection (`ProjectionTarget` y `dependencies`), no revision strings privadas.

No confundir una dependencia semántica real con un orden artificial de bootstrap.

## Reglas congeladas

```text
LEGACY                      REMOVE
ADAPTERS / SHIMS / ALIASES FORBIDDEN
DOBLE CONTRATO              FORBIDDEN
revision -> ProjectionTarget reconstruction REMOVE
expected_source_revision    REMOVE
private projection revision identity REMOVE
```

No reabrir Manager core, Navigation, Users, Tools ni KPI Configuration para resolver el siguiente consumer.
