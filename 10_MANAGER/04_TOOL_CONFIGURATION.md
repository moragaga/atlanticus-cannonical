# Manager — Tool Configuration

Estado: **FROZEN/CURRENT**

## Ownership

Tool Configuration es ADA-specific y permanece en:

```text
scopes/ada/web/tools/configuration
```

Usa infraestructura genérica Atlanticus sin transferir ownership al core.

## Autoridad estructural

Tool Configuration determina qué estructura existe.

Data determina el estado de lo que ya existe.

Una Tool correctamente configurada debe poder montar su UI aunque todavía no existan datos.

La existencia de la aplicación Web no depende de que exista una Tool Configuration publicada.

## Tool kinds

Baseline operacional congelado:

### PROCESS

- ámbito operacional global;
- Components con `layout_role`;
- CENTER obligatorio;
- baseline de alarmas centrado en operación central.

### INTEGRATED OPERATIONS

- sin ámbito global único;
- cada Component declara scope;
- baseline de alarmas considera todos los Components.

## Topología

Component/Subcomponent keys son identidad consumible.

Component es unidad funcional de datos:

```text
1 Component = 1 logical Store/Collector identity
```

Subcomponent:

```text
no Store propio
no Collector propio
no destino KPI propio
```

## Source CURRENT

Tool Configuration publica mediante:

```text
ToolSourceService
SourceStore
SourceSnapshot
SourceReleaseRef
PublishRequest
PublishResult
ConcurrencyToken
HistoryPage
```

Recurso CURRENT:

```text
tools/configuration.json.gz
```

No existe Source identity de dominio basada en `revision`.

## Projection CURRENT

```text
ToolProjectionBuilder
ProjectionTarget
ProjectionStore[ToolConfiguration]
SourceProjectionService[ToolConfiguration]
```

Persistencia durable CURRENT:

```text
tool_projection_to_document
tool_projection_from_document
LocalToolProjectionStore
CosmosToolProjectionStore
```

No existe snapshot privado ni `projection_revision` paralelo.

## Namespace CURRENT

Tool persistence recibe:

```text
AdaStorageNamespace(
    application_namespace,
    tool_namespace,
)
```

Local projection:

```text
<base>/<application>/<tool>/projections
```

Cosmos projection:

```text
partition_key = <application>/<tool>
```

`SourceKey('tools')` no incluye namespace de deployment.

## Resilient persistence composition CURRENT

```text
ToolPersistenceSettings
ToolPersistenceComposition
compose_tool_persistence
```

Providers:

```text
Source     local | blob
Projection local | cosmos
```

Resolution:

```text
resolve_active_tool_projection()
project_current_tool_source()
```

States:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

Runtime puede consumir Projection activa sin requerir Source disponible.

## Legacy removido

No forman parte del contrato Tool Configuration CURRENT:

```text
ToolLifecycleServices
ToolAdministrationService
ToolProjectionWorkflow
ToolConfigurationSourceSnapshot
ToolConfigurationProjectionSnapshot
ToolConfigurationProjectionRepository
ToolConfigurationPublisher
ToolConfigurationSource
expected_source_revision
build_tool_configuration_projection_revision
```

No reintroducir aliases, adapters o shims.

## Manager integration

ADA Configuration Manager consume los contratos genéricos Source/Projection de Tools.

El nuevo package `ada-web-tools-persistence` es una composición reusable de providers; no implica
que cada runtime Manager existente ya haya sido migrado a ese package.

No crear un segundo contrato Manager específico para lograr ese wiring.

## Qualification relevante

Storage namespace:

```text
15 passed
```

Tool Projection persistence observado:

```text
configuration codec  2 passed
projection-local     3 passed
projection-cosmos    5 passed
```

Tool persistence composition:

```text
10 passed
ruff check PASS
ruff format --check PASS
git diff --check PASS
```

Checkpoint CURRENT:

```text
moragaga/atlanticus@21cfb2f11362c1606ad14ff8adc7551948eced6a
```
