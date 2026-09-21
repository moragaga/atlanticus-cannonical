# ADA Generic — Current Composition

Estado: **VERIFIED / GAP IDENTIFIED**

Implementación auditada:

```text
scopes/ada/web/application/ada-generic-application
moragaga/atlanticus@21cfb2f11362c1606ad14ff8adc7551948eced6a
```

## Composición base CURRENT

ADA Generic compone entre otras capacidades:

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

## Tool resolution CURRENT dentro de ADA Generic

El archivo:

```text
ada/web/application/generic/operational_tool.py
```

todavía contiene:

```text
_StartupToolProjectionStore
resolve_current_tool_projection(...)
```

El resolver:

```text
SourceStore
→ select current Source
→ project into in-process store
→ return ProjectionRecord
```

y si Source current no existe:

```text
RuntimeError('Operational Tool source has no current release')
```

Este comportamiento permanece implementado pero ya no representa la dirección final del
bootstrap operacional.

## Infraestructura Tool disponible fuera de Generic Application

CURRENT en `main`:

```text
AdaStorageNamespace

LocalToolProjectionStore
CosmosToolProjectionStore

ToolPersistenceSettings
ToolPersistenceComposition
compose_tool_persistence

resolve_active_tool_projection
project_current_tool_source
```

`resolve_active_tool_projection()` permite leer la Projection durable sin depender de Source.

## __main__ CURRENT

Sigue siendo:

```text
create_application_runtime()
→ run_web_application(runtime)
```

No existe todavía wiring desde settings/environment hacia `ToolPersistenceComposition`.

## Collector

Collector permanece opcional respecto de Generic Application.

No hardcodear Collector ni Cosmos dentro de la composición base.

## Gap CURRENT

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```

Debe reemplazar la dependencia startup in-process de Tool por la composición durable CURRENT,
sin perder la capacidad de levantar la Web cuando no existe Tool o una dependencia externa está
indisponible.
