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

No son sólo etiquetas visuales.

Se utilizan como frontera para:
- datos;
- KPI;
- render;
- routing visual;
- alarmas.

## Component

Component es la unidad funcional de datos:

- 1 Store;
- 1 Collector contract;
- destino KPI;
- baseline/ámbito de alarmas;
- puede contener N Subcomponents.

## Subcomponent

Subcomponent es granularidad visual interna:

- no Store propio;
- no Collector propio;
- no destino KPI;
- sí puede ser target visual independiente de alarma.

## Regla

`N Subcomponents != N Stores != N Collectors`

No fragmentar un Component en infraestructura adicional sin escala real que lo justifique.

## Render vacío

Cosmos/Store vacío es un estado válido.

- configurado + sin dato → `EMPTY`;
- expectativa que no puede resolverse → `NOT_MAPPED`;
- no pertenece a Tool → no render.

## Alarmas

Estado de datos y estado de alarmas son dimensiones independientes.

Un Subcomponent puede:
- data = EMPTY;
- alarm = CRITICAL.

La alarma no determina existencia estructural.

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

El builder decodifica la release exacta y valida la configuración con la validación operacional ADA existente.

No existe snapshot privado de Projection ni `projection_revision` privado como identidad paralela.

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

El `ada-configuration-manager` publicado todavía referencia `ToolLifecycleServices` y `ToolConfigurationManagerWorkflowAdapter`.

KPI Configuration y KPI Definition ya completaron también sus cutovers Source/Projection. Por tanto ya no existe una dependencia de dominio pendiente que justifique postergar el consumer final.

Estado:

```text
ADA-CONFIGURATION-MANAGER-FINAL-GENERIC-CUTOVER
PLANNED / NEXT
```

El ajuste debe ocurrir en el consumer. No adaptar Tools para sostener el contrato antiguo.

## Qualification

Los tests CURRENT para Source/Projection existen en el repositorio.

Su ejecución scoped posterior al cutover Tools permanece:

```text
UNVERIFIED
```
