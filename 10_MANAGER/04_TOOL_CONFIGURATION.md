# Manager — Tool Configuration

Estado: **FROZEN/CURRENT — TRANSVERSAL TOOL CONTRACT CUTOVER CLOSED**

## Ownership

Tool Configuration continúa siendo ADA-specific y permanece en:

```text
scopes/ada/web/tools/configuration
```

Su responsabilidad Web incluye:

```text
ToolConfiguration
BrandingConfiguration integration
ToolSourceService / codecs
Source / Projection composition
persistence composition
editor / callbacks / presentation
```

Los contratos estructurales y de Source compartidos por productos ADA tienen un único owner transversal:

```text
scopes/ada-contracts/tools
package: ada-contracts-tools==1.0.0
namespace: ada.contracts.tools
```

Incluyen, entre otros:

```text
ProcessLayoutRole
ToolConfigurationKind
ToolScope
ToolStructure
ToolComponent
ToolSubcomponent
ToolSubcomponentAddress
ToolSourceConsumption
ToolSourceOperationalParticipation
SourceControlPolicy
ToolDependencyEntry
ToolDependencyManifest
```

`ada.web.tools.configuration` consume esos tipos transversales; no define una copia paralela de ellos.

El backend puede consumir `ada.contracts.tools` cuando intercambia exactamente ese contrato transversal. Un modelo interno backend con estado, ciclo de vida o comportamiento propio puede mantenerse separado y convertir explícitamente en la frontera. Backend no debe depender de `ada.web.tools`.

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

Los value objects compartidos de consumo/participación de Source pertenecen a `ada.contracts.tools`; el servicio de publicación permanece en Tool Configuration Web.

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
Source      local | blob
Projection  local | cosmos
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

## Tool contract Web cutover CLOSED

La release-chain de ADA Generic fue migrada desde el owner duplicado:

```text
ada.web.tools.{enums,errors,structure,validation,sources}
ada-web-tools==0.1.0
```

hacia:

```text
ada.contracts.tools.*
ada-contracts-tools==1.0.0
```

Release-chain calificada:

```text
ada-contracts/tools
    -> ada-web-tools-configuration
    -> projection-local / projection-cosmos
    -> ada-web-tools-persistence
    -> ada-configuration-manager
    -> ada-generic-application
```

El runtime export de ADA Generic confirmó:

```text
ada-contracts-tools  PRESENT
ada-web-tools        ABSENT
```

## Legacy y retiro físico

Para la release-chain ADA Generic, `ada-web-tools` queda **SUPERSEDED**.

El directorio físico:

```text
scopes/ada/web/tools/core
```

permanece temporalmente en el checkout porque referencias de Command Center todavía impiden retirarlo sin cruzar scope.

Estado:

```text
SUPERSEDED as Web owner
BLOCKED for physical retirement
```

No crear alias, adapter o shim de compatibilidad para prolongar su uso.

## Manager integration

ADA Configuration Manager consume los contratos genéricos Source/Projection de Tools y el contrato transversal `ada.contracts.tools`.

`ada-web-tools-persistence` sigue siendo la composición reusable de providers.

No crear un segundo contrato Manager específico para lograr ese wiring.

## Qualification relevante

Cierre local del cutover:

```text
ada-contracts/tools                 10 passed
ada-web-tools-configuration         75 passed
projection-local                     3 passed
projection-cosmos                    5 passed
ada-web-tools-persistence           10 passed
ada-configuration-manager           64 passed
ada-generic-application            205 passed
runtime dependency gate            PASS
release-chain ownership scan       PASS
```

Base remota usada para el incremento:

```text
moragaga/atlanticus@df2a125cf428085419595d8ad164fce0f8d86115
```

La calificación corresponde al working tree local resultante del incremento. No implica que esos cambios estén ya integrados en `main` ni que exista una nueva distribución publicada.
