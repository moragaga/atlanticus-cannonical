# ADA Command Center — Tool Catalog

Estado: **CURRENT V1 / IMPLEMENTED / CLOSED**

## Propósito

Command Center mantiene una visión consolidada y read-only de las Tools que necesita para authoring
asistido y para una futura resolución B.2.

Implementado bajo:

```text
scopes/ada-command-center/backend/tools/catalog
```

## Ownership congelado

ADA Tool Configuration continúa siendo autoridad de:

- `tool_key`;
- display name;
- Tool kind;
- Components;
- Subcomponents;
- relaciones;
- topología.

Command Center Tool Catalog es estado derivado/read-only.

No es:

- Tool authoring;
- segunda source of truth;
- fork de `ToolConfiguration`/`ToolStructure`;
- Cosmos propio de Command Center para duplicar Tool topology.

## Identity CURRENT

`tool_key` es identidad de Tool.

El catálogo exige unicidad dentro de cada snapshot y el consolidator rechaza dos inputs que entreguen
el mismo `tool_key`.

`display_name` es presentación. Forma parte del contenido versionado del snapshot, pero no reemplaza
la identidad por key.

## Contract CURRENT

```text
ToolCatalogEntry
├── tool_key: str
├── display_name: str
├── kind: ToolConfigurationKind
├── source_release_id: SourceReleaseId
└── structure: ToolStructure
```

`structure.tool_key` y `structure.kind` deben coincidir con la entry.

```text
ToolCatalogSnapshot
├── revision: str
├── generated_at_utc: datetime
└── tools: tuple[ToolCatalogEntry, ...]
```

Las Tools se normalizan ordenadas por `tool_key`.

## Revision CURRENT

`revision` es el SHA-256 de una serialización JSON canónica de las entries ordenadas.

Participan:

- `tool_key`;
- `display_name`;
- `kind`;
- `source_release_id`;
- `ToolStructure.to_document()`.

No participa:

- `generated_at_utc`.

Mismo contenido semántico produce la misma revision aunque cambie la hora de generación.

## Inputs CURRENT

```text
ToolCatalogInput
├── input_key: str
├── projection: ProjectionStore[ToolConfiguration]
└── source_key: SourceKey = SourceKey('tools')
```

`input_key` identifica el input dentro del consolidator y debe ser único.

La composición puede inyectar múltiples stores físicos distintos. El package Tool Catalog no conoce
Cosmos directamente.

No asumir un Cosmos global.

## Consolidation CURRENT

```text
configured ToolCatalogInputs
        ↓
get_active(source_key)
        ↓
ToolConfiguration + ToolStructure
        ↓
ToolCatalogEntries
        ↓
ToolCatalogSnapshot
        ↓
replace_current(snapshot)
```

El refresh es all-or-nothing.

Falla sin publicar si cualquier input:

- lanza error al leer;
- no tiene Projection activa;
- entrega payload que no es `ToolConfiguration`;
- no tiene `structure`;
- colisiona en `tool_key`.

No existe `first wins` ni `last wins`.

## Durable state CURRENT

Implementado:

```text
ToolCatalogStore
BlobToolCatalogStore
BlobToolCatalogStoreSettings
```

Settings físicos:

```text
container_name
blob_name
```

`BlobToolCatalogStore` usa `atlanticus.connectivity.storage.StorageClient`.

`get_current()`:

- descarga y decodifica el snapshot;
- retorna `None` si el blob no existe;
- expone error de store si Storage falla.

`replace_current()`:

- sobrescribe el único blob CURRENT;
- usa `application/json`.

## Codec CURRENT

```text
document_type = ada_command_center_tool_catalog
schema_version = 1
```

Documento:

```text
document_type
schema_version
revision
generated_at_utc
tools[]
```

Al leer se reconstruye `ToolStructure` con su contrato CURRENT y se verifica que `revision`
corresponda al payload.

## Failure/LKG semantics V1

V1 no modela availability state por Tool.

No existen en el snapshot:

```text
AVAILABLE
STALE
MISSING
```

Si refresh falla, `replace_current()` no se ejecuta y el blob CURRENT anterior queda sin cambios.

Por tanto, V1 obtiene continuidad mediante **last successfully published CURRENT**, no mediante un
segundo documento LKG ni entries STALE.

La dirección canonical anterior que exigía availability states desde V1 queda
**SUPERSEDED / REFINED**.

Podrán introducirse en un incremento posterior sólo si existe una necesidad operacional concreta.

## History / scheduler

No implementado en V1:

- history de snapshots;
- retention;
- manifest separado;
- cadence;
- retry/backoff;
- scheduler/job;
- startup orchestration;
- metrics específicas del consolidator.

Estas capacidades no son requisito para considerar cerrado el contrato V1.

## Consumer CURRENT — Alarm Configuration authoring

Alarm Configuration ya consume el catálogo mediante:

```text
AlarmToolReferenceReader
```

Produce:

```text
AlarmToolReferenceCatalog
→ Tool
→ Component
→ visible Subcomponent address
```

Conserva:

- `catalog_revision`;
- `source_release_id` por Tool;
- `owner_component_key` real para subcomponentes linked.

Reutiliza operaciones de `ToolStructure`; no duplica la topología.

`STRATEGIC` no se expone como sugerencia de alarmas porque el contrato Tool CURRENT no define esa
proyección.

## Authoring no restrictivo

Catálogo y read model son ayuda de authoring.

No son requisito para persistir una Alarm Configuration intrínsecamente válida.

```text
catalog missing
!=
Alarm Configuration invalid
```

El consumer read model retorna `None` cuando todavía no existe snapshot CURRENT.

## B.2

B.2 permanece PLANNED.

Será un consumidor posterior de:

```text
Alarm Configuration Projection revision
+
Tool Catalog revision
```

No introducir resolución B.2 dentro del Tool Catalog ni dentro del reader de authoring.

## Siguiente foco

Tool Catalog V1 está cerrado.

El siguiente foco no es ampliar este package sino consumir el read model ya disponible en:

```text
ALARM-CONFIGURATION-STRUCTURED-AUTHORING-V1
```
