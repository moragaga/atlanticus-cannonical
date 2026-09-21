# Manager — Source Blob Handoff

Estado: **SOURCE IMPLEMENTED / GENERIC HANDOFF CURRENT**

## Source productivo

Source productivo objetivo:

```text
Azure Blob Storage
```

Local conserva semántica equivalente de desarrollo/QA.

SharePoint/Power Automate pueden permanecer sólo donde consumers explícitos todavía no hayan
migrado; no son autoridad donde Blob ya lo sea.

## Source Core

Source Core cubre:

```text
SourceKey
SourceReleaseId
SourceReleaseRef
immutable releases
manifest
SourceStore
current/concurrency
History
exact reads
integrity verification
```

## Source -> Projection

```text
ProjectionTarget =
    SourceKey
    +
    SourceReleaseRef
    +
    dependencies exactas cuando correspondan
```

`project(target)` ejecuta el target exacto.

## Logical namespace

Container físico y path lógico son conceptos distintos.

ADA CURRENT dispone de:

```text
AdaStorageNamespace
application_namespace
tool_namespace
```

Para Tool:

```text
root_prefix = <application>/<tool>
```

`BlobSourceStore` agrega internamente su layout Source:

```text
sources/<SourceKey>/...
```

La composición no debe pasar `<application>/<tool>/sources` como root, porque duplicaría
responsabilidad interna de `SourceStore`.

Ejemplo:

```text
container físico
└── conciencia_situacional/
    ├── users/
    └── operaciones_integradas/
        └── sources/
```

El container no se infiere del namespace.

## Users boundary

Users no pertenece al handoff Source/Projection.

```text
Users Administration
SEPARATE ADMIN LIFECYCLE
```

`users` pertenece al namespace global de aplicación.

La integración concreta de `AdaStorageNamespace` con el Users store no fue implementada en este
hito; permanece fuera de alcance.

## Tool Projection provider boundary

Tools ahora dispone de Projection durable:

```text
LocalToolProjectionStore
CosmosToolProjectionStore
```

Cosmos separa Tools mediante:

```text
partition_key = <application>/<tool>
```

sin modificar `SourceKey('tools')`.

## Provider composition

CURRENT:

```text
ToolSourceProvider     LOCAL | BLOB
ToolProjectionProvider LOCAL | COSMOS
```

Las selecciones son independientes.

No confundir hosting:

```text
Docker / Azure
```

con provider:

```text
blob / cosmos
```

Azurite/Azure Blob satisfacen el mismo provider lógico Blob.

## Runtime read boundary

Runtime Tool puede leer:

```text
resolve_active_tool_projection()
```

sin consultar Source.

Workflow de proyección usa:

```text
project_current_tool_source()
```

y sí depende de Source current.

## Checkpoint CURRENT

```text
moragaga/atlanticus@21cfb2f11362c1606ad14ff8adc7551948eced6a
```

No reintroducir rutas exact/legacy ni adapters Manager.
