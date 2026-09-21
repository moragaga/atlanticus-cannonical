# ADA Command Center — Current Implementation

Estado: **VERIFIED / UPDATED 2026-09-21**

Corte auditado:

```text
moragaga/atlanticus@4fe03660ad47d105c55167dc583f09be1f395275
```

Parent inmediato:

```text
8e133ad7da3524874add8323315e8c2b3c3f1ee1
```

Tree:

```text
d893128c23939e8a1e8bdd60b5bae64d14983be8
```

## Físicamente en `main`

```text
scopes/ada-command-center/
├── backend/
│   ├── alarms/
│   │   ├── core/
│   │   └── persistence/
│   ├── processes/
│   │   └── alarms-runtime/
│   └── tools/
│       └── catalog/
└── web/
    └── alarms/
        └── configuration/
```

Clasificación:

```text
Backend Alarm Engine                              IMPLEMENTED / CURRENT
Alarm Configuration contract                     IMPLEMENTED / VERIFIED / CURRENT
Alarm Configuration Source/Release               IMPLEMENTED / VERIFIED / CURRENT
Alarm Configuration base Projection              IMPLEMENTED / VERIFIED / CURRENT
Alarm Configuration Manager integration          IMPLEMENTED / VERIFIED / CURRENT
Alarm Configuration document-mode Web surface    IMPLEMENTED / VERIFIED / CURRENT
Command Center Tool Catalog V1                   IMPLEMENTED / VERIFIED / CURRENT
Alarm Tool Reference read model V1               IMPLEMENTED / CURRENT
Structured Alarm Configuration authoring UI      NOT YET IMPLEMENTED
Standalone Command Center application/shell      NOT YET IMPLEMENTED
B.2 ResolvedAlarmConfiguration                   NOT YET IMPLEMENTED
Runtime/Delivery materialization from B.2        NOT YET IMPLEMENTED
Management Projection                            NOT YET IMPLEMENTED
```

## Alarm Configuration CURRENT

Paquete:

```text
scopes/ada-command-center/web/alarms/configuration
```

La unidad editable/publicable es:

```text
AlarmConfiguration
├── rules: tuple[AlarmDefinition, ...]
└── messages: tuple[MessageDefinition, ...]
```

El contrato durable no cambió durante este hito.

La semántica CURRENT permanece:

```text
VALID
!=
FULLY RESOLVED
!=
READY
```

## Tool Catalog V1 CURRENT

Paquete:

```text
scopes/ada-command-center/backend/tools/catalog
```

Contrato:

```text
ToolCatalogEntry
├── tool_key
├── display_name
├── kind
├── source_release_id
└── structure: ToolStructure

ToolCatalogSnapshot
├── revision
├── generated_at_utc
└── tools
```

`tools` se ordena por `tool_key` y los `tool_key` duplicados se rechazan.

`revision` es SHA-256 determinístico sobre el contenido de las entries:

- `tool_key`;
- `display_name`;
- `kind`;
- `source_release_id`;
- `ToolStructure.to_document()`.

`generated_at_utc` no participa en la revisión.

### Consolidator

```text
ToolCatalogInput
├── input_key
├── ProjectionStore[ToolConfiguration]
└── source_key = SourceKey('tools') por defecto
```

`ToolCatalogConsolidator.refresh()`:

1. lee cada Projection activa;
2. exige payload `ToolConfiguration`;
3. exige `configuration.structure`;
4. rechaza `tool_key` duplicado;
5. crea un snapshot completo;
6. sólo entonces ejecuta `store.replace_current(snapshot)`.

No publica parcial si algún input falla.

### Blob store

`BlobToolCatalogStore` usa `StorageClient` y settings explícitos:

```text
container_name
blob_name
```

`get_current()` devuelve `None` cuando el blob aún no existe.

V1 no implementa:

- history;
- LKG separado;
- AVAILABLE/STALE/MISSING;
- scheduler/cadence;
- discovery automático de Tool stores;
- Cosmos propio de Command Center.

El blob CURRENT previo permanece sin cambios cuando el consolidator falla antes de publicar.

## Alarm Tool Reference read model CURRENT

Implementado en:

```text
scopes/ada-command-center/web/alarms/configuration/
src/ada_command_center/web/alarms/configuration/tool_references.py
```

Contrato:

```text
AlarmToolReferenceCatalog
├── catalog_revision
└── tools
    └── AlarmToolReference
        ├── tool_key
        ├── display_name
        ├── kind
        ├── source_release_id
        └── components
            └── AlarmToolComponentReference
                ├── component_key
                ├── display_name
                └── subcomponents
                    └── AlarmToolSubcomponentReference
                        ├── owner_component_key
                        ├── subcomponent_key
                        └── display_name
```

`AlarmToolReferenceReader` depende de `ToolCatalogStore` y:

- retorna `None` si no existe catálogo CURRENT;
- no oculta errores físicos del store;
- reutiliza `ToolStructure.alarm_baseline_component_keys`;
- reutiliza `ToolStructure.alarm_subcomponent_addresses_for_component()`;
- conserva `owner_component_key` para subcomponentes linked;
- omite `STRATEGIC` de las sugerencias porque la proyección de alarmas no está definida para ese kind.

No modifica `AlarmConfiguration.from_document()` ni agrega validación externa al save/publish.

## Qualification observada

### Tool Catalog V1

Ejecutado por el usuario antes de publicar el checkpoint `8e133ad7...`:

```text
pytest                    13 passed
ruff check .              All checks passed!
ruff format --check .     13 files already formatted
```

### Alarm Tool References V1

Ejecutado por el usuario durante integración:

```text
pytest                    32 passed
ruff check .              All checks passed!
ruff format --check .     indicó 1 test por reformatear
```

Luego se indicó ejecutar `ruff format tests/test_tool_references.py` antes del cierre y la
implementación fue publicada en `main@4fe03660...`.

No se capturó en este chat una corrida post-publicación de los tres gates sobre exactamente ese SHA.
Por tanto, la qualification funcional/lint previa está **VERIFIED**, mientras la qualification
completa del checkpoint exacto queda **UNVERIFIED** hasta una corrida explícita si se requiere como
gate formal.

## Base histórica ya disponible

Alarm Engine conserva Journey/Evidence/Occurrence y demás hechos operacionales ya auditados.

Este hito no modificó:

- Alarm Engine Domain Model;
- Runtime lifecycle;
- persistence/recovery;
- Analytics boundary.
