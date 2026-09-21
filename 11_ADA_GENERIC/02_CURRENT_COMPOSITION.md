# ADA Generic — Current Composition

Estado: **CLOSED / VERIFIED / CURRENT**

Implementación auditada:

```text
scopes/ada/web/application/ada-generic-application
moragaga/atlanticus@bc8eafc21a65e3f9aff044c232e2562cd490c49f
```

## Composición base CURRENT

ADA Generic compone capacidades como:

```text
branding
navigation
operational header
alarm surfaces
content state
operational render binding
operational state
runtime experience
source consumption / operational participation
time status
global indicators
session/runtime Web
```

La composición base existe independientemente de Collector.

## Bootstrap operacional CURRENT

Entrada:

```text
AdaGenericSettings
```

Cadena:

```text
environment / .env
→ Tool persistence settings
→ optional Storage client
→ optional Tool Projection Cosmos client
→ ToolPersistenceComposition
→ resolve_operational_tool_projection()
→ resolve_active_tool_projection()
```

No existe la ruta startup legacy basada en:

```text
_StartupToolProjectionStore
resolve_current_tool_projection
```

Esos símbolos no están presentes en `main` para este cierre.

## Tool resolution

Estados:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

`READY` exige además validación operacional ADA de la `ToolConfiguration`.

Estados no READY mantienen disponible la Web base con diagnóstico.

No existe fallback silencioso a Source ni a otro provider.

## Collector runtime wiring

Cuando Tool está `READY` y KPI Delivery Cosmos está configurado:

```text
Tool Projection
→ ToolStructure
→ create_operational_kpi_collector()
→ attach_operational_kpi_collector()
→ create_web_application()
```

La conexión KPI Delivery usa configuración de consumo separada de Tool Projection.

La ausencia completa de configuración KPI no elimina la Web.

Collector permanece lazy respecto del polling.

## Operational Render CURRENT

`OperationalRenderBinding` es estructural:

```text
ToolStructure
→ one OperationalComponentBinding per ToolComponent
```

Fue removido el acoplamiento con `ComponentStoreSnapshot`.

Collector no depende de `operational-render-binding`.

## Data delivery boundary

Collector publica browser stores existentes:

```text
1 ToolComponent
→ 1 dcc.Store
```

Subcomponents no crean store propio.

ADA Generic termina su responsabilidad genérica en esa superficie de datos.

No construye un body obligatorio para cada Tool.

## Estado del hito

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
CLOSED / VERIFIED / CURRENT

ADA-GENERIC-COLLECTOR-RUNTIME-WIRING
CLOSED / VERIFIED / CURRENT

ADA-GENERIC-OPERATIONAL-RENDER-RUNTIME-CONTRACT
CLOSED / VERIFIED / CURRENT

ADA-GENERIC-STAGE-1
CLOSED / VERIFIED / CURRENT
```
